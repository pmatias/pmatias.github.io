---
layout: post
title:  "Drush now supports including makefiles from git repositories"
date:   2015-01-07 09:15:38
categories: git drush
share: true
comments: true
---

One of my clients works with multiple makefiles for hundreds of different projects, and we are currently working on a better way to organise several dozens of makefiles. One approach that we've found to be very practical for us is the following _example_ structure:

```
api = 2
core = 7.x

; platform.make contains core drupal and several main modules/themes/libraries
; that we need to have available for all of our projects
includes[platform] = "platform.make"

; sub_platform.make contains specific modules/themes/libraries for
; specific requirements on a different set of projects
includes[subplatform] = "sub_platform.make"

; [...]
```

## The problem
The only problem with this set up is that our makefiles are distributed in several *private* git repositories (divide and conquer: this way we can have better control on what project/patches are inside on the different types of platforms) so getting the makefiles from raw URLs is off the table since it will need some kind of authentication, and we're not going to write nasty stuff to make that happen.

## The solution
The 'includes' directive supports git repositories. This way, we get to assemble the main makefile the way we need it (also we get to define the core directive in the remote makefile that we want, and not the main one, giving us more control on Drupal versions).

## How it works
Drush make now supports (at the time of writing this post, it's only available on the master branch of the drush project) the following sentences in the makefiles:

```
includes[remote][makefile] = 'drupal.make'
includes[remote][download][type] = "git"
includes[remote][download][url] = "git@github.com:organisation/repository.git"
; Branch could also be tag or revision,
; it relies on the standard Drush git download feature.
includes[remote][download][branch] = "7.x"
```

Also, despite our reasons of needing this feature, it's a natural feature that 'includes' could use, since it supports local files, relative path local files and remote URLs.

You can go ahead and give it a spin from the [Drush](https://github.com/drush-ops/drush) project page.
