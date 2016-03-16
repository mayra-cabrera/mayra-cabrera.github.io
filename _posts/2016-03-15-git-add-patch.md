---
layout: post
title: Git add patch
description: The most powerful git feature you're not using yet
modified: 2016-03-15
tags: [git]
comments: true
---


Often when we are ready to commit our changes, we'll use `git add` or `git add --all`, and this, as we expected, will **add all of our files** into the stage area. But what about if we don't want that? What about if we just want to add **one change** into the stage area, and leave the remaining changes for other commits? Well as you'll guessed there's a flag for that!

```
$ git add --patch
```

When running the former, Git will split the changes in each file into small pieces and present them one at a time, prompting you for how to proceed :). Is basically like `git rebase --interactive` but for adding changes. 
