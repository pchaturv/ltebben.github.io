---
layout: post
title:  "Creating this website"
date:   2018-07-08 18:30:00 -0500
categories: 
  - programming
tags: 
  - programming
---

Install [Ruby](https://www.ruby-lang.org/en/downloads/)

Install bundler
```scss
$ gem install bundler
```

Create an empty github repo titled <username>.github.io

In your local repository, run
```scss
$ bundle exec jekyll new .
```

Open Gemfile and delete
```scss
"jekyll", "3.3.0"
```
and uncomment
```scss
gem "github-pages", group: :jekyll_plugins
```

To use a different theme (in my example [hydeout](https://github.com/fongandrew/hydeout))
add to Gemfile
```scss
gem "jekyll-theme-hydeout"
```
change in _config.yml
```scss
theme: minima
```
to 
```scss
remote_theme: fongadrew/hydeout
```
and add the following plugins
```scss
plugins:
  - jekyll-feed
  - jekyll-paginate
  - jekyll-remote-theme
  - github-pages
```

On the command line run 
```scss
$ bundle install
```
to install the dependencies

Run 
```scss
$ bundle exec jekyll serve
```
to deploy the site to localhost:4000

To move blog posts of the Home page:
create new directory /blog/
add an index.html file in that directory with the front matter: 
```scss
---
layout: index
title: Blog
permalink: /blog/
---
```
in _config.yml add the following lines:
```scss
paginate: 5
paginate_path: '/blog/page:num'
sidebar_blog_link: '/blog'
```

in Gemfile, add
```scss
gem "jekyll-paginate"
```