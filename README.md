# GitHub Pages

## [My personal website on GitHub](https://thanapoom21.github.io)

GitHub Pages are public web pages for users, organizations, and repositories, that are freely hosted on GitHub's `github.io` domain or on a custom domain name of your choice. GitHub Pages are powered by Jekyll behind the scenes, so they are a great way to host your Jekyll-powered website for free.

The site will automatically be generated by GitHub Pages when pushing source files. GitHub Pages works equally well for regular HTML content, simply because Jekyll treats files without front matter as static assets. It is easy to push generated HTML w/o any further setup and headaches.

---

## The github-pages gem

GitHub have provided the [github-pages](https://github.com/github/pages-gem) gem which is used to manage [Jekyll and its dependencies on GitHub Pages](https://pages.github.com/versions/). Using it in projects means that when deploying the site to GitHub Pages, there is very little chances for unexpected differences between various versions of the gems.

Note that GitHub Pages runs in `safe` mode and only allows [a set of whitelisted plugins](https://help.github.com/en/articles/configuring-jekyll-plugins#default-plugins).

To use the currently-deployed version of the gem in your project, add the following to you `Gemfile`:

```
source "https://rubygems.org"

gem "github-pages", group: : jekyll_plugins
```

Be sure to run ```bundle update``` often.