---
layout: page
title: How-to
categories: navigation
---

* *Notes* are markdown files in the repo's root directory.
* Earlier, notes used to be *Blog Posts*, dated markdown files under `_posts` directory.
* Kurippu is for notes. Create/edit notes in root directory itself.

## New Note

* Just an entry belonging to an existing topic? Ex: Git, `git.md`. Add it to the respective file.


## New Topic

* Create a markdown file in the repo root. ( *name-of-new-topic.md*)

* Add yaml front matter

```

    ---
    layout: page
    title: "Title of the NEW Topic"
    ---

```

* Should page come in the navigation bar?: Add `categories: navigation` to yaml front matter


## Organizing notes

Unlike blog posts, pages don't have much of inbuilt tag/category support. Should add the needed catogories in `_config.yml`. (I Added *navigation*)


## Redirects

[github/jekyll redirects](https://help.github.com/articles/redirects-on-github-pages/)
