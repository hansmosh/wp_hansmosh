---
ID: 65
post_title: >
  A little git trick for developing in a
  shared environment
author: hansmosh
post_date: 2016-03-22 12:46:56
post_excerpt: ""
layout: post
permalink: http://blog.hansmosh.com/?p=65
published: false
---
> tl;dr -- Add `0 5 * * 1 git config --global --remove-section user` to crontab in order to clear globally configured git user every Monday morning.

I have found that it's not uncommon to find yourself wanting to commit some new code on a server that is used by other people. We can get into the "best practices" around this kind of development later, but for now we'll admit to our faults and try to figure out the best way to support this.

The problem is when you go to commit your code and you see:

    user@host ~/path/to/my/project$ git commit -m "This is not a good commit message"
    
    *** Please tell me who you are.
    
    Run
    
        git config --global user.email "you@example.com"
        git config --global user.name "Your Name"
    
    to set your account's default identity.
    Omit --global to set the identity only in this repository.
    

On a shared computer, I don't really want to set up any git config with my email and name, especially not globally. But I'm going to do it anyway with the intention of removing the config when I'm done... like that will happen.

*So let's use a cron job to remove the git config for us.* Edit your cron jobs with `crontab -e` and add these two lines somewhere in the file:

    # Periodically remove global git user
    0 5 * * 1 git config --global --remove-section user 
    

Now the globally configured git user will be cleared every Monday morning at 5 am and I don't have to worry about someone else committing as me next week.