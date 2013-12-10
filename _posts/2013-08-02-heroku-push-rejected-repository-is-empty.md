---
layout: post
title: 'FIX: Git push rejected, repository is empty'
date: '2013-08-02T03:50:00+08:00'
tags: [heroku, git]
---

You know the repository is not empty but no matter how many times you enter `git push` this error occurs.




```
    !     Push rejected, repository is empty
    To git@heroku.com:my-heroku-site.git
    ! [remote rejected] master -> master (pre-receive hook declined)
```

The problem is your repository only contains files beginning with a `.` (dot) like in this repository:

```
$ git ls-files
.vendor_urls
```

The solution is to add another file which doesn't start with a dot, like a `Procfile`.
