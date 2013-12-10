---
layout: post
title: Don' t merge a file but be able to push it
date: '2013-02-15T06:34:00+08:00'
tags: [git, git merge, .gitattributes, merge conflict]
---

So I wanted to not merge a specific file. But adding it to `.gitignore` is not a solution since I won't be able to push that file to my remotes.



I use SASS as my CSS preprocessor so I have my css file expanded on development and compressed on production in my `config.rb` file.

{% highlight text %}
# development
environment=:development

#production
environment=:production
{% endhighlight %}

When I merge `production` with `development` I always get conflicts with my css file. So what's the way to solve this?

## Use .gitattributes

Try adding the following to your `.gitattributes` (production branch)

{% highlight text %}
*.css merge=ours
{% endhighlight %}

Then define configurations for merge `ours`:

{% highlight text %}
$ git config merge.ours.name 'always keep ours'
$ git config merge.ours.driver 'touch %A'
{% endhighlight %}

If your look into your `.git/config` file it shall contain:

{% highlight text %}
[merge "ours"]
    name = always keep ours
    driver = touch %A
{% endhighlight %}

After this try doing a merge and you won't get conflicts anymore.
