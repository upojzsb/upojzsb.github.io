---
layout:     post
title:      Modifications of This Blog
subtitle:   Blog
date:       2000-01-01
author:     UPOJZSB
header-img: img/post-bg-black.jpg
catalog: true
tags:
    - Blog
---

# Overall

This blog is developed based on [Qiubaiying](https://github.com/qiubaiying/qiubaiying.github.io), and as a just-for-fun project, I have made some modifications. This post is used to record  modifications to my blog.

# Configuration

## _config.yml

- Change **url** parameter from *http* into *https*.

 This will affect all links related to **site.url**, such as **[sitemap.xml](https://upojzsb.github.io/sitemap.xml)**

# Theme

## Dark theme

To make my blog more eye-friendly, I decided to change the theme to a dark theme. Introducing [css](https://github.com/upojzsb/upojzsb.github.io/commit/794f389fdc859d4674b4a58103c81971189f97ca) in [head.html](https://github.com/upojzsb/upojzsb.github.io/commit/a5b1edbace7fba25dcddc0a12e12ff9c4c035ddd) to achieve this effect.

## Therme Change

Support therme changing betweeen light mode and dark mode. Add a checkbox [therme-switch.html](.) with [switch.css](.), and include therme-switch.html in both [page.html](.) and [post.html](.), include switch.css in [head.html](.) and include both light-mode CSS and dark-mode CSS in [head.html](.)
