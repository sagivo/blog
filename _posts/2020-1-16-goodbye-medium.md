---
layout: post
title: Goodbye Medium
description: (link to the github repo)
categories: []
keywords: []
---

Today I moved my blog away from [Medium](http://medium.com).  
Initially it was a great friendship. I loved the fonts, design and the easy discovery option.  
But then things went south very fast.  

First, it was super hard to configure custom domain. Later Medium stopped any support for domains for new customers.  

Then, Medium started charging people money to read posts and blocked my content with annoying marketing popup, asking people to signup first. Basically used my content to promote them.  
![medium paywall](/assets/medium1.png)

The final stroke was the aggressive new design that leave no place for content.  
![medium page](/assets/medium2.png)  

So I decided to go all open source and in few nights configured a [jekyll blog](https://jekyllrb.com/).  
Jekyll is awesome! It's an open source blogging template on top of Ruby.  
I'm hosting it for free on top of [Github Pages](https://github.com/pages-themes) who has built in support for it.  

Looks like I'm not the only one. The web is full with stories of [people](https://wptavern.com/freecodecamp-moves-off-of-medium-after-being-pressured-to-put-articles-behind-paywalls) who [left](https://news.ycombinator.com/item?id=20745313) [Medium](https://medium.com/@dan_abramov/why-my-new-blog-isnt-on-medium-3b280282fbae) [becase](https://sylviesoul.com/reasons-you-should-leave-medium-now) of [the](https://arslan.io/2017/07/30/why-i-left-medium-and-moved-back-to-my-own-domain/) [recent](https://hackernoon.com/why-i-quit-my-medium-membership-909909657ba) [chages](https://praxis.fortelabs.co/why-im-leaving-medium/).

### Transition
The move is fairly simple and in order to export your content from Medium to Jek
- Download your medium content into zip file from the settings page. 
- Run the Ruby script in [this article](https://medium.com/@jameshamann/displaying-medium-posts-on-your-jekyll-website-7eef230309e4).
- Configure [jekyll](https://jekyllrb.com/).
- Deploy your new blog for free using Github Pages.
- Enjoy a whole new add-free world for your content.

### Next steps
The basic theme is not great, I would love to upgrade the design someday, till them - focus on the content :)

