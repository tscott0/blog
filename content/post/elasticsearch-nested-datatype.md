+++
date = "2018-10-31T21:00:00Z"
title = "Nested datatypes in Elasticsearch"
draft = false

+++

<!--more-->

### Create the index

`PUT /products`
```json
{
    "mappings" : {
        "_doc" : {
            "properties" : {
                "tag" : { "type" : "nested" }
            }
        }
    }
}
```

### Add some products

A tomato is red and round.

`PUT /products/_doc/1`
```json
{
  "title" : "tomato",
  "tag" : [
    {
      "name" : "red"
    },
    {
      "name" : "round"
    }
  ]
}
```

An apple is green and round (mostly)

`PUT /products/_doc/2`
```json
{
  "title" : "apple",
  "tag" : [
    {
      "name" : "green"
    },
    {
      "name" : "round"
    }
  ]
}
```

### Searching for round things

`POST /products/_search`
```json
{
  "query": {
    "nested": {
      "path": "tag",
      "query": {
        "bool": {
          "must": [
            { "match": { "tag.name": "round" }}
          ]
        }
      }
    }
  }
}
```
Returns both items!
```json
{
    "took": 2,
    "timed_out": false,
    "_shards": {
        "total": 5,
        "successful": 5,
        "skipped": 0,
        "failed": 0
    },
    "hits": {
        "total": 2,
        "max_score": 0.6931472,
        "hits": [
            {
                "_index": "products",
                "_type": "_doc",
                "_id": "2",
                "_score": 0.6931472,
                "_source": {
                    "title": "apple",
                    "tag": [
                        {
                            "name": "green"
                        },
                        {
                            "name": "round"
                        }
                    ]
                }
            },
            {
                "_index": "products",
                "_type": "_doc",
                "_id": "1",
                "_score": 0.6931472,
                "_source": {
                    "title": "tomato",
                    "tag": [
                        {
                            "name": "red"
                        },
                        {
                            "name": "round"
                        }
                    ]
                }
            }
        ]
    }
}
```

### Searching for red things

`POST /products/_search`
```json
{
  "query": {
    "nested": {
      "path": "tag",
      "query": {
        "bool": {
          "must": [
            { "match": { "tag.name": "red" }}
          ]
        }
      }
    }
  }
}
```
Returns just the tomato. This apple is green, remember.
```json
{
    "took": 2,
    "timed_out": false,
    "_shards": {
        "total": 5,
        "successful": 5,
        "skipped": 0,
        "failed": 0
    },
    "hits": {
        "total": 1,
        "max_score": 0.6931472,
        "hits": [
            {
                "_index": "products",
                "_type": "_doc",
                "_id": "1",
                "_score": 0.6931472,
                "_source": {
                    "title": "tomato",
                    "tag": [
                        {
                            "name": "red"
                        },
                        {
                            "name": "round"
                        }
                    ]
                }
            }
        ]
    }
}
```

### Searching for a green apple

`POST /products/_search`
```json
{
  "query": {
    "bool": {
      "must": [
        {
          "match": {
            "title": "apple" 
          }
        },
        {
          "nested": {
            "path": "tag", 
            "query": {
              "bool": {
                "must": [ 
                  {
                    "match": {
                  "tag.name": "green"
                    }
                  }
                ]
              }
            }
          }
        }
      ]
}}}
```

Returns just the apple
```json
{
    "took": 2,
    "timed_out": false,
    "_shards": {
        "total": 5,
        "successful": 5,
        "skipped": 0,
        "failed": 0
    },
    "hits": {
        "total": 1,
        "max_score": 0.98082924,
        "hits": [
            {
                "_index": "products",
                "_type": "_doc",
                "_id": "2",
                "_score": 0.98082924,
                "_source": {
                    "title": "apple",
                    "tag": [
                        {
                            "name": "green"
                        },
                        {
                            "name": "round"
                        }
                    ]
                }
            }
        ]
    }
}
```