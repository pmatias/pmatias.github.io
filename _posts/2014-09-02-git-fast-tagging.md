---
layout: post
title:  "Git fast tagging"
date:   2014-09-02 09:15:38
categories: git
share: true
---

Let's say I have a new stable version to release and I want to back up the current one.
<br />
The first step would be to tag the stable release with a _backup_ name.

<div class="highlight">
		$ git tag stable-2014-09-02 stable
</div>

Now we delete the stable tag

<div class="highlight">
		$ git tag -d stable
</div>

Now it's time to mark the current commit(or any commit) as the stable tag(make sure to `git log` to know which commit hash you want)

<div class="highlight">
		$ git tag -a stable -m "Tagging stable version" 60f7196
</div>

Next step is to delete the stable branch in the remote server

<div class="highlight">
		$ git push origin :refs/tags/stable
</div>

All done, it's time to push the local tags

<div class="highlight">
		$ git push --tags 
		<br/>
		Counting objects: 1, done.
		<br/>
		Writing objects: 100% (1/1), 171 bytes | 0 bytes/s, done.
		<br/>
		Total 1 (delta 0), reused 0 (delta 0)
		<br/>
		To git@github.com/xxxx/xxxx.git
		<br/>
 			* [new tag]         stable -> stable
		<br/>
 			* [new tag]         stable-2014-09-02_1 -> stable-2014-09-02
</div>

Quick, dirty and easy :D
