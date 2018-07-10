---
layout: post
title:  "Creating this website"
date:   2018-07-08 18:30:00 -0500
categories: 
  - programming
tags: 
  - programming
---

I ran into a lot of issues creating this website and had to consult numerous tutorials to get all the details I needed so I decided to create my own tutorial! Below are the steps I followed to get this site up and running on GitHub pages.

Install [Ruby](https://www.ruby-lang.org/en/downloads/)

Install bundler
```scss
$ gem install bundler
```

Create an empty github repo titled *username*.github.io

In your local repository, initialize the jekyll project by running
```scss
$ bundle exec jekyll new .
```

If you want to host your site on Github Pages
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
gem "jekyll-paginate"
```
In _config.yml change
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

On the command line install the dependencies by running 
```scss
$ bundle install
```

Now deploy the site to localhost:4000 by running 
```scss
$ bundle exec jekyll serve
```

If this fails, try manually installing the packages on the command line:
```scss
gem install jekyll-feed
gem install jekyll-paginate
gem install jekyll-remote-theme
gem install github-pages
gem install jekyll-theme-hydeout
```

To move blog posts off the Home page to a blog page:
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

To create blog posts, add files to the _posts/ directory using the naming convention YYYY-MM-DD-title.md

To create new pages, add new markdown files to the root directory. Adding the front matter sidebar_link:true will add a link in the sidebar to the page. Adding the front matter permalink: /*link*/ creates a permalink to this page.