---
layout: post
title:  "Yes, you CAN revert a git push --force on Github"
date:   2015-01-10 17:15:38
categories: git
share: true
comments: true
excerpt_separator: <!--more-->
---
## A small introduction

We're always making mistakes (_we're only humans_), specially when you work on IT. Unless you do something that involves having the lives of others in your hands, making mistakes is OK. The important thing about it is to learn from them, and try to solve them as efficient as possible.
<!--more-->
So, the first rule of the _make-a-mistake-club_ is: [Don't Panic](http://en.wikipedia.org/wiki/The_Hitchhiker%27s_Guide_to_the_Galaxy).

As I was saying, making mistakes when you work on IT is common (after all, it's a trial and error business), it's expected, and you're encouraged to make them. If you don't believe that this is true, then you're maybe in the wrong career.

Lots of IT companies don't understand this premise, and it's a long topic that I may or may not cover on another post. But, for now, let's just focus on the title.

## The scenario

You are heavily working on a new release on your project, or maybe just bug fixing. After a long day of coding, destroying, debugging and/or trying to figure out what the fuck is going on _up there_. You're done, you finished, and everything is working again, hurray!. Now it's time to clean up.

Surely there will be lots of useless commits to remove from your work before you merge (or pull request) your changes into the main branch.

So you _git rebase_ your branch, squashing, picking, removing.

Then you push.

Your push gets rejected because of the inconsistency of the commit history.

You git push --force your branch, and your push.default behaviour is not set, so it _defaults_ to _matching_.

You overwrite the master branch (and maybe other main branches that you had in your local repository) with a two months old code.

**Oops**.

Take a deep breath, sit back (or stand back, if you work on a standing desktop), and **don't panic**.

This is one of the reasons why working with distributed version control systems is so good: you can _undo_ things.

## The solution

You will need one of this items on the list to do that:

- Someone from your team who has the latest version of those main branches on their machines.
- Another local copy on your machine (ideal, but most unlikely).
- The full SHA of the latest commit you need to restore.

I'd say that the first two scenarios are very unlikely to be met (if you meet one of them, then it's just force pushing those specific branches, and problem solved), so let's go for the third one.

When you force pushed your changes to Github, something like this happened:

```
$ git push --force
Counting objects: 3, done.
Delta compression using up to 4 threads.
Compressing objects: 100% (2/2), done.
Writing objects: 100% (2/2), 231 bytes | 0 bytes/s, done.
Total 2 (delta 1), reused 0 (delta 0)
To git@github.com:your-organisation/your-important-repo.git
 + #{OLD_SHA}...#{NEW_SHA} very-important-branch -> very-important-branch (forced update)

```

See that #{OLD_SHA} reference? it will be our friend here. But you will need the full SHA reference to make this happen, because we're going to play a little with Curl in a bit.

Now, head to https://github.com/your-organisation/your-important-repo/commit/#{OLD_SHA} and you'll be able to grab your full SHA reference code from there.

This is the part where the beautiful Github API comes into action, but first, since our Github account is set to use 2FA (because we're all big responsible professionals here :P), you will need to retrieve your token.

From the Github [getting started guide](https://developer.github.com/guides/getting-started/):

```
curl -i -u <your_username> -H "X-GitHub-OTP: <your_2fa_OTP_code>" \
    -d '{"scopes": ["repo"], "note": "getting-started"}' \
    https://api.github.com/authorizations
```


_your_2fa_OTP_code_ is the temporal code you get from your mobile 2FA app (or SMS).

The result will be the proper token you will need for the next step.

```
curl -u <your_username> -H 'Authorization: token <your_just_generated_token>' \
--request PATCH https://api.github.com/repos/your-organisation/your-important-repo.git/git/refs/heads/master \
--data '{"sha": "#{OLD_FULL_SHA}", "force": true}'
```

This will _force push_ to the latest lost commit, restoring the full lost history. #WIN

## Lesson learnt

A few next steps about what we did in this post:

- Be always explicit with your command line (e.g. git push --force _remote_server_ _branch_ instead of just git push -f. And be careful with the aliases!).
- Check that your your global push.default behaviour is set to _current_ (this will push only the current branch you're on instead of everything).
- The Internet is your very best friend when something like this goes south.
- Don't Panic!

**Update 14-01**: Here are some nice [25 tips for intermediate git users](https://www.andyjeffries.co.uk/25-tips-for-intermediate-git-users/).
