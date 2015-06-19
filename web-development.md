---
layout: page
title: Web Development (Jekyll, Bootstrap)
---

## Integrating Bootstrap with Jekyll

* *Include CSS*: include the CDN link to `bootstrap.min.css` in `_includes/head`, or download css and link. 

* *Edit Layouts* to include bootstrap container etc.

## Jekyll + Bootstrap SASS/SCSS + customization

* Download *Bootstrap Sass*. Copy `assets/stylesheets/bootstrap` directory to jekyll's `_sass` (or whatever sass root). This will have all the components etc.

* Copy `_bootstrap.scss` as `bootstrap.scss` in jekyll's css directory (`./css`)
* Edit `css/bootstrap.scss` and add an empty yaml front matter.

{% highlight yaml linenos %}

---
---
/*!
* Bootstrap v3.3.5 (http://getbootstrap.com)
* Copyright 2011-2015 Twitter, Inc. ... */

{% endhighlight%}

Otherwise it won't be compiled to css, rather directly copied to the _site.


### Customizing

If bootstrap is included as a plain css file, create another sass/css file `bootstrap-custom` and override whatever class directly. (Do include the resulting `css` file in `_includes/head`)

If bootstrap is included as sass:

* New file under css: `bootstrap-custom.scss`. Include this file in `_includes/head`
* Add blank front-matter. First two lines of `---`.
* *Import* whatever

{% highlight sass linenos %}

// Core variables and mixins
@import "bootstrap/variables";
@import "bootstrap/mixins";

{% endhighlight%}

* Customize and use.

[http://getbootstrap.com/css/#example-usage](http://getbootstrap.com/css/#example-usage) has some mistakes. Correct version:

{% highlight sass linenos %}

.wrapper { @include make-row(); }

.content-main {
  @include make-lg-column(8);
}

.content-secondary {
 @include  make-lg-column(3);
 @include  make-lg-column-offset(1);
}


{% endhighlight%}
