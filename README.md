# GitHub Pages

## [My personal website on GitHub](https://thanapoom21.github.io)

GitHub Pages are public web pages for users, organizations, and repositories, that are freely hosted on GitHub's `github.io` domain or on a custom domain name of your choice. GitHub Pages are powered by Jekyll behind the scenes, so they are a great way to host your Jekyll-powered website for free.

The site will automatically be generated by GitHub Pages when pushing source files. GitHub Pages works equally well for regular HTML content, simply because Jekyll treats files without front matter as static assets. It is easy to push generated HTML w/o any further setup and headaches.

---

## The github-pages gem

GitHub have provided the [github-pages](https://github.com/github/pages-gem) gem which is used to manage [Jekyll and its dependencies on GitHub Pages](https://pages.github.com/versions/). Using it in projects means that when deploying the site to GitHub Pages, there is very little chances for unexpected differences between various versions of the gems.

Note that GitHub Pages runs in `safe` mode and only allows [a set of whitelisted plugins](https://help.github.com/en/articles/configuring-jekyll-plugins#default-plugins).

To use the currently-deployed version of the gem in your project, add the following to you `Gemfile`:

```ruby
source "https://rubygems.org"

gem "github-pages", group: : jekyll_plugins
```

Be sure to run ```bundle update``` often.

## Project Page URL Structure

Sometimes it's nice to preview Jekyll site before pushing `gh-pages` branch to GitHub. The subdirectory-like URL structure GitHub uses for Project Pages complicates the proper resolution of URLs. In order to assure your site builds properly, use the handy URL filters:

```html
<!-- For styles with static names... -->
<link href="{{ "/assets/css/style.css" | relative_url }}" rel="stylesheet">
<!-- For documents/pages whose URLs can change... -->
[{{ page.title }}]("{{ page.url | relative_url }}")
```

The site can be viewed on localhost from the site root, when GitHub generates the pages from `gh-pages` branch all the URLs will resolve properly.

Findings:

When serving files locally on a specific port, HTML, CSS, and JS seem to work well together. However, a bug occured when pushing files to GitHub, what appears on the console was files not found or 404, even when testing with different approaches.

Tips: Try to install gem with only one method. DO NOT COMBINE methods or ruby will get confused.

- Fork and clone from a repo.
- Add gem using ```gem install just-the-docs``` or add ```gem "just-the-docs"``` directly on Jekyll site's Gemfile. Then add ```theme: "just-the-docs"```.
- Add remote_theme: owner/repository , for exameple, ```remote_theme: pmarsceill/just-the-docs```

Tips: baseurl and url key in _config.yml file can cause issue when pushing to GitHub Pages. If baseurl and url are provided but the site does not render properly, try remove the value and keep empty ```""``` and the style should render properly.