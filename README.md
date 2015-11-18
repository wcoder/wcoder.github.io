Tech blog
======================

The site uses [Jekyll](http://jekyllrb.com), a static site generator. GitHub Pages, where the site is hosted, natively supports Jekyll so every time someone pushes to this repository, the website will be built and updated. For hosting it yourself, see [Setting up a local copy of the website(#setting-up-a-local-copy-of-the-website).

Setting up a local copy of the website
--------------------------------------

All the documentation is available here:
 - [Using Jekyll with Pages](https://help.github.com/articles/using-jekyll-with-pages/)
 - [Deploying Jekyll to GitHub Pages](http://jekyllrb.com/docs/github-pages/)

For run safe `github-pages` version locally:
```
bundle exec jekyll serve
```

**Note:** you can add the `--watch` option when running `jekyll serve` to let Jekyll watch for file changes, which means the site will be rebuilt when a file is modified.

**Note 2:** on case-insensitive file systems like on Windows and Mac OS X you'll run into redirect loops for some URLs. The workaround is to disable redirects locally by removing the `gems: jekyll-redirect-from` entry from `_config.yml`.

The site should now be running locally...

Repository structure
--------------------

 - `_includes` - *special folder* contains snippets that can be included via `{% include file.html %}` in other pages
 - `_layouts` - *special folder* contains the layouts that are shared between pages. Layouts can be inherited, the root layout is `base.html`.
 - `_posts` - *special folder*, contains the source pages for the blog section, see [Writing a blog post](#writing-a-blog-post)
 - `_site` - the output of the generated site is stored here by default, this folder only exists after Jekyll built the site

Writing a blog post
-------------------

Blogging is very easy with Jekyll. Simply add a new Markdown file to the `_posts` folder following the file name convention: `YEAR-MONTH-DAY-title.md`

Make sure to not include special characters in the file name. The blog entry's publishing date is automatically extracted from the file name.