---
layout: post
title: "Programming from a Cafe with Github"
date: 2013-07-02 17:07
comments: true
categories: 
---
I do most of my programming from cafes and until today I've faced a
major annoyance - a lot of public wifis are configured to block ssh
access. Unfortunately this is how I connect with Github, so I haven't
able to push any code changes while working at the cafes. Today I found
a fix for this [on StackOverflow][], quoting [this article from
Github][] - all you need to do is configure your computer to use port
443 for GitHub access, which is the default HTTPS port and is usually
not blocked. Essentially you're making an SSH connection over the port
generally used for HTTPS. 1. Test to see whether you can make SSH
connections over the HTTPS port. In the terminal, run:
`$ ssh -T -p 443 git@ssh.github.com` You should get the following
response: Hi *username*! You've successfully authenticated, but GitHub
does not provide shell access. 2. Open '\~/.ssh/config' using your text
editor (I use Sublime Text 2): ` ` `subl ~/.ssh/config ` 3. Paste in the
following: ` Host github.com Hostname ssh.github.com Port 443` 4. Test
that it works: ` $ ssh -T git@github.com` You should get the following
response: Hi *username*! You've successfully authenticated, but GitHub
does not provide shell access. And that's it. Now you should be able to
work with Git in public places. What I am curious about is whether there
are security issues when doing this, and how to make it work for heroku
as well. I guess that'll be for a future post.

  [on StackOverflow]: http://stackoverflow.com/questions/7953806/github-ssh-via-public-wifi-port-22-blocked
  [this article from Github]: https://help.github.com/articles/using-ssh-over-the-https-port