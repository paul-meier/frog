# Frog

<a href="http://www.flickr.com/photos/doug88888/4717363945/" title="Happy Green frog by @Doug88888, on Flickr"><img src="http://farm5.staticflickr.com/4070/4717363945_b73afd78a9.jpg" width="300" height="300" alt="Happy Green frog"></a>
<br />
[<img src="https://raw.github.com/dcurtis/markdown-mark/master/png/48x30-solid.png">][Markdown]
[<img src="http://getbootstrap.com/2.3.2/assets/ico/favicon.png">][Bootstrap]
[<img src="http://racket-lang.org/logo.png" width="30" height="30">][Racket]
[<img src="http://pygments.org/media/icon.png" width="40" height="40">][Pygments]

<sub><em><a href="http://www.flickr.com/photos/doug88888/4717363945/">Frog image by @Goug8888</a>, used under Creative Commons license [Attribution-NonCommercial-ShareAlike 2.0 Generic](http://creativecommons.org/licenses/by-nc-sa/2.0/)</em></sub>

[![Build Status](https://travis-ci.org/greghendershott/frog.png?branch=master)](https://travis-ci.org/greghendershott/frog)

# Overview

Frog is a static web site generator written in [Racket][].

You write content in [Markdown][] or [Scribble][]. You generate
files. To deploy, you push them to a GitHub Pages repo (or copy them
to Amazon S3, or whatever).

Posts get a variety of automatic blog features.

You can also create non-post pages.

The generated site uses [Bootstrap][], which is [responsive][],
automatically adapting to various screen sizes.

Yes, it's very much like Octopress and others. But Frog doesn't
require installing Ruby. Installing Racket is typically waaaay
simpler and faster.

The only non-Racket part is optionally using [Pygments][] to do syntax
highlighting.

Q: "Frog"?
A: Frozen blog.

> **NOTE:** This has been tested on Mac OS/X and Ubuntu; I'm using it for
> [my blog][]. CAVEAT: It has _not_ yet been tested on Windows--it's
> likely that the path handling isn't exactly right.

# Quick Start

## Installing Frog

1. Install [Racket 5.3.4 or newer](http://racket-lang.org/download/).

2. Install Frog: `$ raco pkg install frog`.
   
    > **NOTE:** If you build Racket from HEAD, make sure you pull after 2013-Oct-07 (to get commit `5eee140`) and rebuild. Otherwise `raco pkg install frog` will error.

3. Optional: Install Pygments if you want syntax highlighting for
   fenced code blocks: `$ sudo easy_install --upgrade Pygments`.
   
    > **NOTE:** If that fails, first install `easy_install` -- e.g. `$ sudo apt-get install python-setuptools` -- and try again.

    > **NOTE:** Why `--upgrade`? You probably want the most recent version of Pygments because new languages are constantly being added. For example, Racket is supported starting in Pygments 1.6.


## Starting a new blog project

Creating a new blog project is 3 easy steps:

```sh
# 1. Create a subdir
$ mkdir frog-project

# 2. Go there
$ cd frog-project

# 3. Tell Frog to create default files and dirs
$ raco frog --init
Configuration /tmp/frog-project/.frogrc not found; using defaults.
Creating files in /tmp/frog-project/:
/tmp/frog-project/.frogrc
/tmp/frog-project/_src/About.md
/tmp/frog-project/_src/page-template.html
/tmp/frog-project/_src/post-template.html
/tmp/frog-project/_src/posts/2012-01-01-a-2012-blog-post.md
/tmp/frog-project/css/
/tmp/frog-project/js/
/tmp/frog-project/img/
Project ready. Try `raco frog -bp` to build and preview.
```

You can go ahead and build/preview this to get a feel for the default
starting point:

```sh
# Build and preview it
$ raco frog -bp
Using configuration /tmp/frog-project/.frogrc
13:28 Done generating files
Your Web application is running at http://localhost:3000/.
Stop this program at any time to terminate the Web Server.
# The home page should launch in your web browser.
# Switch back to the command prompt and type C-c to quit:
^C
Web Server stopped.
```


### Project file tree

Here is the file tree that `raco frog --init` creates for you and Frog
expects:

```sh
project/
  # Files provided by you:
  .frogrc       # see next section of README for details
  _src/
    page-template.html  # lets you define the page layout
    post-template.html  # lets you define the <article> layout
    posts/
      # You'll create these using `raco frog -n <post-title>`
      2013-01-01-a-blog-post.md
      2013-02-15-another-blog-post.md
      ...
    # Zero or more other .md files for non-post pages.
    # May be in subdirs.
  css/
    bootstrap.css            #\
    bootstrap.min.css        # get these files
    bootstrap-theme.css      # from getbootstrap.com
    bootstrap-theme.min.css  #/
    pygments.css             # used to style code elements from Pygments
    custom.css               # other styles you provide; may be empty
  js/
    bootstrap.js             # get these files
    bootstrap.min.js         # from Bootstrap
  img/
    feed.png
  favicon.ico
```

Here are the files created by Frog when you run `raco frog -b` to
(re)build the blog:

```sh
project/
  index*.html
  sitemap.txt
  tags/
  feeds/
  # Post pages in subdirs.
  # Exact layout depends on `permalink` in `.frogrc`.
  blog/
    2013/
      01/
        a-blog-post-title/
          index.html
        ...
    2013/
      02/
        another-blog-post-title/
          index.html
        ...
  ...
```

> **NOTE**: Although the Frog `/example` project has copies for
> example purposes, for your own project you should get the
> official/latest Bootstrap 3 files directly from [Bootstrap][].


> **TIP**: To design a Bootstrap 3 "theme", try
> [Bootstrap Magic](http://pikock.github.io/bootstrap-magic/).


> (Although <http://bootswatch.com/> has some ready-made themes,
> they're for Bootstrap 2.x, and to use them with Bootstrap 3 you'll
> need to hand-tune the CSS.)


> **TIP**: For examples of `pygments.css` code highlighting styles see
> <https://github.com/richleland/pygments-css>.


### Per-project configuration: .frogrc

`raco frog --init` creates a `.frogrc` file in your project directory:

```sh
# Required: Should NOT end in trailing slash.
scheme/host = http://www.example.com

title = My Awesome Blog
author = The Unknown Author

# Whether to show the count of posts next to each tag in sidebar
show-tag-counts? = false

# Pattern for blog post permalinks
# Optional: Default is "/{year}/{month}/{title}.html".
# Here's an example of the Jekyll "pretty" style:
permalink = /blog/{year}/{month}/{day}/{title}/index.html
# There is also {filename}, which is the `this-part` portion of
# your post's YYYY-MM-DD-this-part.md file name. This is in case
# you don't like Frog's encoding of your post title and want to
# specify it exactly yourself, e.g. to match a previous blog URI.

# Should index page items contain full posts -- more than just the
# portion above "the jump" <!-- more --> marker (if any)?
index-full? = true

# Should feed items contain full posts -- more than just the portion
# above "the jump" <!-- more --> marker (if any)?
feed-full? = true

# How many posts per page for index pages?
posts-per-page = 10

# How many items to include in feeds?
# Older items in excess of this will not appear in the feed at all.
max-feed-items = 20

# Decorate feed URIs with Google Analytics query parameters like
# utm_source ?
decorate-feed-uris? = true

# Insert in each feed item an image bug whose URI is decorated with
# Google Analytics query parameters like utm_source ?
feed-image-bugs? = true

# Replace links to tweets with embedded tweets?
# In Markdown, must be auto-links alone in a pargraph (blank lines
# above and below), for example:
#
# <https://twitter.com/racketlang/status/332176422003163138>
#
auto-embed-tweets? = true

# Try to automatically link symbols in Markdown ```racket fenced code
# blocks, to Racket documentation?
racket-doc-link-code? = true

# Try to automatically link Markdown of the form `symbol`[racket] to
# Racket documentation? i.e. This is similar to the @racket[] form in
# Scribble.
racket-doc-link-prose? = true
```

## Creating blog posts

A typical workflow:

1. Create a new post with `raco frog -n "My Post Title"`. The
name of the new `.md` file is displayed to stdout.

2. Edit the `.md` file in your preferred plain text editor.

3. Regenerate your site and preview it with `raco frog -bp`. (You might
repeat steps 2 and 3 a few times until you're satisfied.)

4. Deploy. If you're using [GitHub Pages][], you can commit and push
to deploy to your real site. If you're using some other method, you
can copy or rsync the files to your static file server.

> **TIP**: If you use Emacs, try markdown-mode. Although Markdown is
> _really_ simple, markdown-mode makes it even more enjoyable.

# More details

## Posts

You create new posts in `_src/posts`. They should be named
`YYYY-MM-DD-TITLE.md` and need to have some meta-data in the first few
lines.

You can do `raco frog -n "My Title"` to create such a file
easily. This will also fill in the required meta-data section. The
markdown file starts with a code block (indented 4 spaces) that must
contain these three lines:

```markdown
    Title: A blog post
    Date: 2012-01-01T00:00:00
    Tags: foo, bar, tag with spaces, baz

Everything from here to the end is your post's contents.

If you put `<--! more -->` on a line, that is the "above-the-fold"
marker. Contents above the line are the "summary" for index pages and
Atom feeds.

<!-- more -->

Contents below `<!-- more -->` are omitted from index pages and Atom
feeds. A "Continue reading..." link is provided instead.

```

`Title` can be anything.

`Date` must be an ISO-8601 datetime string: `yyyy-mm-ddThr:mn:sc`.

`Tags` are optional (although you have to include the `Tags:` part).

> The tag `DRAFT` (all uppercase) causes the post `.html` file _not_
> to be generated.

### Automatic post features

Posts are automatically included in various index pages and feeds.

Posts with _any_ tag go on the home page `/index.html`, in an Atom feed
`/feeds/all.atom.xml`, and in an RSS feed `/feeds/all.rss.xml`.

Posts for each tag go on an index page `/tags/<tag>.html`, in an Atom
feed `/feeds/<tag>.atom.xml`, and in an RSS feed
`/feeds/<tag>.rss.xml`.

The default template `post-template.html` provides:

- Twitter and Google+ sharing buttons.
- Disqus comments.

The default template `page-template.html` (used for _all_ pages, not
just post pages) also provides:

- Twitter follow button.
- Google Analytics tracking.

## Non-post pages

You can put other `.md` files in `_src`, and in subdirs of it (other
than `_src/posts`). They will be converted to HTML pages.  For
example, `_src/About.md` will be `/About.html` in the site.

> **NOTE**: Non-post pages are _not_ included in any automatically
> generated index pages or feeds. You can manually add them to the nav
> bar by editing that porition of `page-template.html`.

## sitemap.txt

A `/sitemap.txt` file (for web crawlers) is automatically generated
and includes all post and non-post pages. (It does _not_ include index
pages for tags.)

## Templates

Frog uses the Racket
[`web-server/templates`](http://docs.racket-lang.org/web-server/templates.html)
system based on `scribble/text` `@`-expressions. This means that the
files are basically normal HTML format, with the ability to use `@` to
reference a template variable --- or indeed to "escape" to arbitrary
Racket code.

In contrast to most templating systems, you have a full programming
language available -- Racket -- should you need it. However most of
what you need to do will probably very simple, such as the occasional
`if` or `when` test, or perhaps defining a helper function to minimize
repetition.

### Page template: `_src/page-template.html`

The `_src/page-template.html` template specifies an `<html>` element
used by Frog to generate every page on your site.

Anything in the file that looks like `@variable` or `@|variable|` is a
template variable supplied by Frog.  Most of these should be
self-explanatory from their name and from seeing how they are used in
the default template. Specifically:

- `contents`: The contents of the page.
- `title`: The title of the page (for `<title>`)
- `description`: The description of the page (for `<meta>` content element)
- `keywords`: The keywords for the page (for `<meta>` keywords element)
- `uri-path`: The path portion of the URI, e.g. `/path/to/file.html`
- `full-uri`: The full URI, e.g. `http://example.com/path/to/file.html`
- `atom-feed-uri`: The full URI to the Atom feed
- `rss-feed-uri`: The full URI to the RSS feed
- `tag`: If this an index page, `tag` is the name of the index (such
  as "All Posts") or ("Posts tagged foo"), else `tag` is `#f`.
- `tags-list-items`: HTML with a `<li>` for every tag on the blog, suitable for putting in a `<ul>`. Each `<li>` has a link to that tag's index page.
- `tags/feeds`: HTML that has, for each tag, a link to its index page
  and a link to its Atom feed.

### Post template: `_src/post-template.html`

The `_src/post-template.html` template determines how blog posts are
laid out, on page that are dedicated to one post. The default template
defines an `<article>` element.

For pages that are blog posts, the result of `post-template.html`
becomes most of the `@|contents|` variable in `page-template.html`. In
other words, the post template is effectively nested in the page
template. (They are two separate templates so that the page template
can also be used for pages that are not blog post pages.)

    +---------------------------+
    | page-template             |
    |                           |
    |       +---------------+   |
    |       | post-template |   |
    |       +---------------+   |
    |                           |
    +---------------------------+

> **NOTE**: This template does _not_ control how a blog post is laid
> out on an index page like `/index.html` or
> `/tags/<some-tag>.html`. Why?  The main purpose of this template is
> to specify things like Disqus comments, Tweet and +1 sharing
> buttons, and older/newer links --- things that only make sense in
> the context of pages dedicated to one blog post.

Anything in the file that looks like `@variable` or `@|variable|` is a
template variable supplied by Frog.  Most of these should be
self-explanatory from their name and from seeing how they are used in
the default template. Specifically:

- `title`: The title of the post
- `uri-path`: The path portion of the URI, e.g. `/path/to/file.html`
- `full-uri`: The full URI, e.g. `http://example.com/path/to/file.html`
- `date+tags`: The date and tags of the post
- `content`: The content of the post
- `older-uri`: The URI of the next older post, if any, or `#f`
- `older-title`: The title of the next older post, if any, or `#f`
- `newer-uri`: The URI of the next newer post, if any, or `#f`
- `newer-title`: The title of the next newer post, if any, or `#f`

### Widgets

In addition to the variables described above for each template, some
predefined functions are available for templates to use: "widgets".

Anything a widget can do, you could code directly in the
template. There's no magic. But widgets minimize clutter in the
templates. Plus they make clearer what are the user-specific
parameters (as opposed to putting stuff like `<!-- CHANGE THIS! -->`
in the template).

For example, `@google-analytics["UA-xxxxx" "example.com"]` returns
text for a `<script>` element to insert Google Analytics tracking
code. You supply it the two user-specific pieces of information, which
it plugs into the boilerplate and returns.

Likewise there are widgets for things like Twitter and Google+ share
buttons, Twitter follow button, Disqus comments, older/newer post
links.

See [`widgets.rkt`][] for the complete list. See the
[example page template][] and [example post template][] for usage
examples.

> **NOTE**: If you'd like to add a widget, pull requests are welcome!

## Code blocks

Frog optionally uses [Pygments][] to do syntax highlighting. When
using fenced code blocks, you can specify a language (as on
[GitHub][GHFM]):

    ```language
    some lines
    of code
    ```

That `language` is given to Pygments as the lexer to use.

For example this:

    ```js
    /**
     * Some JavaScript
     */
    function foo()
    {
        if (counter <= 10)
            return;
        // it works!
    ```

Yields this using the `js` lexer:

```js
/**
 * Some JavaScript
 */
function foo()
{
    if (counter <= 10)
        return;
    // it works!
```

And this:

    ```racket
    #lang racket
    ;; Finds Racket sources in all subdirs
    (for ([path (in-directory)])
      (when (regexp-match? #rx"[.]rkt$" path)
        (printf "source file: ~a\n" path)))
    ```

Yields this using the `racket` lexer:

```racket
#lang racket
;; Finds Racket sources in all subdirs
(for ([path (in-directory)])
  (when (regexp-match? #rx"[.]rkt$" path)
    (printf "source file: ~a\n" path)))
```

The colors are controlled by your `css/pygments.css` file. There are
[examples of many styles](https://github.com/richleland/pygments-css).

If you use larger font sizes, code may wrap and get out of alignment
with the line numbers. To avoid the wrapping, add the following to
your `css/custom.css`:

```css
/* When highlighted code blocks are too wide, they wrap. Resulting in the */
/* line numbers column's rows not lining up with the code rows. Prevent */
/* wrapping. */
pre {
    white-space: pre;
    width: inherit;
}
```

> **NOTE**: I have a soft spot for [Pygments][] because it's actually
> the first existing open source project to which I contributed. I added
> a lexer for the [Racket][] language. More importantly it has lexers
> for tons of languages and is used by things like GitHub (via
> [pygments.rb][]), BitBucket, and so on. Plus, it fits the spirit of
> static web site generation better than JavaScript options like
> [SyntaxHighlighter][].


# Scribble source files

Sources for posts (and for non-post pages) may also be [Scribble][]
`.srcbl` files.

See the [example Scribble post][] and
[example Scribble non-post page][] for more information.

> **NOTE**: `raco frog -n` creates `.md` files, only. You will need to
> create the `.scrbl` file for a new post manually, following the
> naming convention.

# Bug reports? Feature requests?

Please use [GitHub Issues][].


[my blog]: http://www.greghendershott.com
[Racket]: http://www.racket-lang.org
[Markdown]: http://daringfireball.net/projects/markdown/syntax
[Scribble]: http://docs.racket-lang.org/scribble/index.html
[Bootstrap]: http://getbootstrap.com/
[responsive]: https://en.wikipedia.org/wiki/Responsive_web_design
[Pygments]: http://pygments.org/
[pygments.rb]: https://github.com/tmm1/pygments.rb
[SyntaxHighlighter]: http://alexgorbatchev.com/SyntaxHighlighter/
[GitHub Pages]: https://help.github.com/articles/user-organization-and-project-pages
[GitHub Issues]: https://github.com/greghendershott/frog/issues
[GHFM]: https://help.github.com/articles/github-flavored-markdown#syntax-highlighting
[`widgets.rkt`]: https://github.com/greghendershott/frog/blob/master/frog/widgets.rkt
[example page template]: https://github.com/greghendershott/frog/blob/master/example/_src/page-template.html
[example post template]: https://github.com/greghendershott/frog/blob/master/example/_src/post-template.html
[example Scribble post]: https://github.com/greghendershott/frog/blob/master/example/_src/posts/2013-06-19-a-scribble-post.scrbl
[example Scribble non-post page]: https://github.com/greghendershott/frog/blob/master/example/_src/A-Non-Post-Scribble-Page.scrbl
