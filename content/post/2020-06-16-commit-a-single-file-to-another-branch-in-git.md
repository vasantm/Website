---
title: commit a single file to another branch in git
author: Vasant
date: '2020-06-16'
slug: commit-a-single-file-to-another-branch-in-git
categories:
  - TIL
tags:
  - til
  - git
  - technical
  - git
subtitle: ''
summary: 'How to commit a single file to another branch'
authors: []
lastmod: '2020-06-16T13:01:38-04:00'
featured: yes
image:
  caption: ''
  focal_point: ''
  preview_only: no
projects: []
---
Let's say you're working on your branch `trunk-style` and want to commit the features you've created  and commit to `main`[^1]  repository. You can do this using `git cherry-pick` like so:

```
# on branch trunk
git add <file>
# Make a note of the commit id
git commit -m "Updated style to include ascii-art" 
git checkout main
git cherry-pick <commit-id>
git push origin main
```

I learned this from [StackOverflow](https://stackoverflow.com/a/42467121/2747709)

[^1]: From now on calling all my master repos `main`, at least locally in git, always had an issue with master/slave terminology. Will follow-up with a post on how to do this.