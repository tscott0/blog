+++
date = "2017-02-25T14:07:20Z"
title = "Configuring Hugo"
draft = true

+++

I started following the excellent [Hugo Quickstart Guide](https://gohugo.io/overview/quickstart/) and wanted to document the experience here. This will one be series of short posts as create a simple blog.

<!--more-->
I'm looking for a blog that
- is lightweight,
- works well on mobile,
- is easy to maintain, and
- provides good support for technical posts.

### Syntax highlighting
In order to show exactly how I'm building this blog, I'll need a way to show some code. Hugo is very easy to get up and running but I want to show the content of config files and some raw Markdown too.

Fortunately, Hugo makes syntax highlighting a breeze. There are two approaches, documented [here](https://gohugo.io/extras/highlighting/). I'm opting for a server-side approach using [Pygments](http://pygments.org) in an attempt to keep the blog lightweight. Client-side syntax highlighters typically use Javascript.

~~~
body {
  font-family: "Noto Sans", sans-serif;
}
~~~

### Summaries
I noticed very quickly that the "summary" of a post, the part that appears on the homepage for your blog, included a lot more of the content than I wanted.

<!--more-->

https://gohugo.io/content/summaries/
