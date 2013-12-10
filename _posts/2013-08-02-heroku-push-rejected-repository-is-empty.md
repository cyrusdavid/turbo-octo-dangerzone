---
layout: post
title: 'FIX: Git push rejected, repository is empty'
date: '2013-08-02T03:50:00+08:00'
tags: [heroku, git]
---

You know the repository is not empty but no matter how many times you enter `git push` this error occurs.




{% highlight text %}
!     Push rejected, repository is empty
To git@heroku.com:my-heroku-site.git
! [remote rejected] master -> master (pre-receive hook declined)
{% endhighlight %}

The problem is your repository only contains files beginning with a `.` (dot) like in this repository:

{% highlight text %}
$ git ls-files
.vendor_urls
{% endhighlight %}

The solution is to add another file which doesn't start with a dot, like a `Procfile`.
