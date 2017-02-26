+++
date = "2017-02-25T14:07:20Z"
title = "Configuring Hugo"
draft = false

+++

I started following the excellent [Hugo Quickstart Guide](https://gohugo.io/overview/quickstart/) and wanted to document the experience here. This will one in a series of short posts as I create this blog.

<!--more-->
I'm looking for a blog that...

- is lightweight,
- works well on mobile,
- is easy to maintain, and
- provides good support for code blocks.

# Syntax highlighting
In order to show exactly how I'm building this blog, I'll need a way to show some code. Hugo is very easy to get up and running but I want to show the content of config files and some raw Markdown. In future I'll want to write about Go too.

Fortunately, Hugo makes syntax highlighting a breeze. There are two approaches, documented [here](https://gohugo.io/extras/highlighting/). I'm opting for a server-side approach using [Pygments](http://pygments.org) in an attempt to keep the blog lightweight. Client-side syntax highlighters typically use Javascript and I want to avoid putting any stress on the browser.

## Installing Pygments
_Assuming Pip is installed_
```bash
$ sudo pip install Pygments
```

Check that `pygmentize` is on the path (`Ctrl-C` to exit)
{{< highlight bash >}}
$ pygmentize
{{< / highlight >}}

## Configuring Pygments
I added the following to my config.toml
```ini
# Use Pygments when using traditional code fences with ```
PygmentsCodeFences = true

# The Pygments theme
PygmentsStyle = "friendly"
```

## Examples
### CSS
```css
body {
  font-family: "Noto Sans", sans-serif;
}
```

### Go with line numbers
{{< highlight go "linenos=inline" >}}
func (b *Band) String() string {
	return fmt.Sprintf("%v", *b)
}
{{< / highlight >}}

### Markdown with line 3 highlighted
{{< highlight md "hl_lines=3" >}}
# How to ride a bike

- Get on top of the bike.
- Put your feet on the pedal.
- Make the pedal turn.
{{< / highlight >}}

# Summaries
I noticed very quickly that the "summary" of a post, the part that appears on the homepage for your blog, included a lot more of the content than I wanted. Summaries also don't render the content as you might expect, formatting is often lost. Ideally I would be able to demarcate exactly where I wanted the summary to finish.

At first I tried using a horizontal rule to end the summary. In Markdown a horizontal line can be done with three dashes `---`. This didn't work so I decided to check the docs.

I learned a couple of important lessons from the [Hugo docs](https://gohugo.io/content/summaries/):

1. By default the summary will contain the first 70 characters of a post.
2. Inserting `<!--more-->` into a post will allow me to manually define the split, just what I was looking for.

{{< highlight html >}}
This is the summary.
<!--more-->
This is the body of the post.
{{< / highlight >}}

# CSS
I decided to fork the [Beautiful Hugo theme](https://github.com/1138-4EB/beautifulhugo) so I could make minor tweaks to the CSS as I go. I've only made very minor changes so far, mainly to padding.

Once I'd created the fork on [GitHub](https://github.com/tscott0/beautifulhugo), I then removed the original theme from the `/themes` directory and checked out my own. Now I can make changes to CSS as I go.
