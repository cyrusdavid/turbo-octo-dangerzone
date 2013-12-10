---
layout: post
title: ANSI Colors on your Windows Command Line
date: '2013-02-13T05:12:00+08:00'
tags: [ANSI,windows,command line,console colors,ANSI escape code,ANSICON]
image:
  feature: CBfpgTg.png
---

So you want those ANSI escaped sequences to be parsed into colors on your Windows Command Line? Not a problem.


Here's what it looks like before:

![colors not parsed on command line prompt](http://i.imgur.com/CBfpgTg.png)

and after having ANSICON installed:

![Colored command prompt on windows](http://i.imgur.com/rQYHlv2.png)

## Get ANSICON

1. <a href="http://adoxa.3eeweb.com/ansicon/" rel="nofollow">Get a copy</a> of ANSICON (<a href="http://adoxa.3eeweb.com/ansicon/dl.php?f=ansicon" rel="nofollow">or Latest here.</a>).

2. Extract it to `C:\Ansicon`

3. Open your command line and cd to that directory then execute `ansicon -i`

{% highlight text %}
C:
cd C:\Ansicon
ansicon -i
{% endhighlight %}

4. Restart your command line prompt and test if it worked

## MSysGit

When using it on MSysGit it might not work. Check the properties of your shortcut and make sure the target location looks similar:

{% highlight text %}
C:\Windows\SysWOW64\cmd.exe /c ""C:\Program Files (x86)\Git\bin\sh.exe" --login -i"
{% endhighlight %}

It has to have that `C:\Windows\SysWOW64\cmd.exe` at the beginning.
