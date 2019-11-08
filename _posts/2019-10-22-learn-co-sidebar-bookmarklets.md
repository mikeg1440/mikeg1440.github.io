---
layout: post
title: Hide Learn.co's Sidebar with a Bookmarklet
date: '2019-10-22 15:12:11 -0400'
permalink: hide-learn-sidebar-with-a-bookmarklet
categories: JavaScript
---
<head>
  <style>
    a{font-weight: bold;}
    .highlighter-rouge{ color: black; padding: 1px;background-color: chartreuse;}
    code {font-size: medium;}
  </style>
</head>


#### Just wanted to share this for anyone who had the same thought that I did, you can either manually add a bookmark and paste the following code in or drag and drop the links that are in this text.  For anyone who is not that familiar with bookmarklets you can find some good info [HERE](https://repl.it/talk/learn/BOOKMARKLETS/14218)

  **NOTE** I only tested this on Chromium so it should work on Chrome as well and I'm reasonably certain it will work on Firefox.  The only browser that may not work is Internet Explorer but let's be honest, IE is a dumpster fire and encourage you to uninstall immediately and choose one of the browsers mentioned above.

## Hide the sidebar

  This is the code to hide the sidebar, it also moves the friends widget over as well.

```
javascript:(function(){$("section.site-sidebar")[0].style.display="none"; document.getElementById('js--region-main').style.right=0; $("section.site-widget")[0].style.right=0;})();
```


## Show the hidden sidebar again

  To make the sidebar visible again you make another Bookmarklet with this code.

```
javascript:(function(){$("section.site-sidebar")[0].style.display=""; document.getElementById('js--region-main').style.right="236px"; $("section.site-widget")[0].style.right="263px";})();
```



## Add via Drag and Drop

  To install the bookmarklet via drag and drop then you can follow these steps.

  1. Make sure your browser displays the bookmark bar
    - For Google Chrome or Chromium browsers:
      - press CRTL-Shift-B. The bookmarks bar should appear or disappear immediately;
      - You could also click the options icon on your top right, ![Firefox Menu Button](https://support.start.me/hc/article_attachments/360002353565/Iopt.PNG) and mouse over to the sixth option 'Bookmarks'. If you find a checkmark there, you're all set.
    - For Firefox, Internet Explorer and Pale Moon users, there is no shortcut available. You can still right-click the certificate in the        address bar, to toggle the bookmarks bar.
    - In Safari you can toggle the Favorites Bar under View > Hide Favorites Bar or by pressing ⇧⌘B.
  2. After your bookmarks are visible just drag and drop these links to your bookmarks
    - [Hide](javascript:(function(){$("section.site-sidebar")[0].style.display="none"; document.getElementById('js--region-main').style.right=0; $("section.site-widget")[0].style.right=0;})();)
    - [Show](javascript:(function(){$("section.site-sidebar")[0].style.display=""; document.getElementById('js--region-main').style.right="236px"; $("section.site-widget")[0].style.right="263px";})();)

### And that's it!  Just goto any learn page that has the sidebar and then click the `Hide` bookmark to hide it and the `Show` bookmark to return to normal.  Also you can rename them whatever you like, it doesn't have to be what I choose.

<hr>

#### I actually added a folder and Named it Flatiron and moved both `Show` and `Hide` bookmarklets under that so it works like this.

  ![Example of how I set up the bookmarklets](https://i.imgur.com/1fvjSPU.gif)
