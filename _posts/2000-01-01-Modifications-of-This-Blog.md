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

To make my blog more eye-friendly, I decided to change the theme to a dark theme. Introducing [CSS](https://github.com/upojzsb/upojzsb.github.io/commit/794f389fdc859d4674b4a58103c81971189f97ca) in [head.html](https://github.com/upojzsb/upojzsb.github.io/commit/a5b1edbace7fba25dcddc0a12e12ff9c4c035ddd) to achieve this effect.

## Theme Switching Implementation

### Overview

This project involved adding a theme-switching feature to a website, allowing users to toggle between light and dark modes. The work included creating a switch component, integrating it into HTML files, and ensuring proper CSS styling for both modes.

### Steps and Implementation Details

1. **Creating the Theme Switch Component:**

   - **File Created:** `therme-switch.html`
   - **Description:** This file contains the HTML structure for a theme-switching checkbox. The checkbox allows users to toggle between light and dark themes.
   - **Code Reference:** [therme-switch.html](https://github.com/upojzsb/upojzsb.github.io/commit/cd6bfb9c21d751dbb170807fd5d494b9858ae9b5)

2. **Styling the Switch:**

   - **File Created:** `switch.css`
   - **Description:** This CSS file styles the checkbox to resemble the theme-switching component found in iOS settings, with a round switch and smooth transitions.
   - **Code Reference:** [switch.css](https://github.com/upojzsb/upojzsb.github.io/commit/afcdf7de065dcb246dafafa7a22c58a337332371)

3. **Integrating the Theme Switch:**

   - **Files Updated:** `page.html` and `post.html`
   - **Description:** The `therme-switch.html` was included in these HTML files to ensure the theme-switching component appears on both the main page and individual posts.
   - **Code Reference:** [page.html and post.html](https://github.com/upojzsb/upojzsb.github.io/commit/0e8d4ae1987e2d872235922e4526f0f216e99703)

4. **Including CSS Files in Head:**

   - **File Updated:** `head.html`
   - **Description:** This file includes references to `switch.css`, `light-mode.css`, and `dark-mode.css` in the `<head>` section, ensuring proper styling for both light and dark themes.
   - **Code Reference:** [head.html](https://github.com/upojzsb/upojzsb.github.io/commit/0e8d4ae1987e2d872235922e4526f0f216e99703)

### Summary of Work

- **Implementation:** The theme-switching functionality was primarily developed with the assistance of ChatGPT. This included creating the theme switch component, styling it, and integrating it into the existing HTML structure.
- **Testing:** The solution was tested to ensure that the theme-switching component works as expected. The checkbox defaulted to dark mode and toggled between light and dark modes successfully.
- **Outcome:** The theme-switching feature was successfully integrated, allowing users to seamlessly switch between light and dark themes. The checkbox was styled to match modern UI elements and positioned correctly within the webpage.

### Acknowledgements

The majority of the work was facilitated through ChatGPT, which provided guidance on implementing and integrating the theme switch. The final adjustments and modifications were made to tailor the solution to specific requirements.
