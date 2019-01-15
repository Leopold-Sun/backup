---
title: hexo-layout
date: 2018-12-24 15:41:20
tags: Hexo
categories: Hexo
description:
keywords: hexo, hexo layout
---

### Hexo layout
```
.                    #hexo root layout
├── .deploy          #hexo deploy to github
├── public           #hexo g outputs static pages
├── scaffolds        #contains scaffolds of post,page and draft.
├── scripts          #user-defined + extension layout
├── source           #post source layout inside which the contents will be deployed in github repo
|   ├── _drafts      #drafts
|   └── _posts       #posts
├── themes           #theme
├── _config.yml      #global configuration
└── package.json     #application information containing information and dependencies relative to hexo
```

<!-- more  -->

### Theme layout
```
.
├── _config.yml                             # next theme configuration
├── languages                               # language and menu name conf
│   ├── default.yml
│   ├── en.yml
│   ├── zh-Hans.yml
│   └── zh-tw.yml
├── layout                                  # site layout conf (you can define your conf/swig/scripts here)
│   ├── archive.swig
│   ├── category.swig
│   ├── _custom                             # Define html page templates (global) and cover the defalut
│   │   ├── header.swig
│   │   └── sidebar.swig
│   ├── index.swig
│   ├── _layout.swig                        # the layout.swig with highest priority
│   ├── _macro                              # Define html page templates (global) and cover the defalut
│   │   ├── post-collapse.swig
│   │   ├── post-copyright.swig
│   │   ├── post.swig
│   │   ├── reward.swig
│   │   ├── sidebar.swig
│   │   └── wechat-subscriber.swig
│   ├── page.swig                           #the page.swig with highest priority
│   ├── _partials                           # Define html page templates (partial) and cover the defalut
│   │   ├── comments.swig
│   │   ├── footer.swig
│   │   ├── head
│   │   │   ├── custom-head.swig
│   │   │   └── external-fonts.swig
│   │   ├── header.swig
│   │   ├── head.swig
│   │   ├── page-header.swig
│   │   ├── pagination.swig
│   │   ├── search
│   │   │   ├── localsearch.swig
│   │   │   ├── swiftype.swig
│   │   │   └── tinysou.swig
│   │   ├── search.swig
│   │   └── share
│   │       ├── add-this.swig
│   │       ├── baidushare.swig
│   │       ├── duoshuo_share.swig
│   │       ├── jiathis.swig
│   │       └── mod_share.swig
│   ├── post.swig                           # the post.swig with highest priority
│   ├── schedule.swig                       # the schedule.swig with highest priority
│   ├── _scripts                            # Define html page templates (partial) and cover the defalut
│   │   ├── boostrap.swig
│   │   ├── commons.swig
│   │   ├── pages
│   │   │   └── post-details.swig
│   │   ├── schemes
│   │   │   ├── gemini.swig
│   │   │   ├── mist.swig
│   │   │   ├── muse.swig
│   │   │   └── pisces.swig
│   │   ├── third-party
│   │   └── vendors.swig
│   ├── tag.swig                            #the tag.swig with highest priority
│   └── _third-party
│       ├── analytics
│       │   ├── analytics-with-widget.swig
│       │   ├── application-insights.swig
│       │   ├── baidu-analytics.swig
│       │   ├── busuanzi-counter.swig
│       │   ├── cnzz-analytics.swig
│       │   ├── facebook-sdk.swig
│       │   ├── firestore.swig
│       │   ├── google-analytics.swig
│       │   ├── index.swig
│       │   ├── lean-analytics.swig
│       │   ├── tencent-analytics.swig
│       │   ├── tencent-mta.swig
│       │   └── vkontakte-api.swig
│       ├── comments
│       │   ├── changyan.swig
│       │   ├── disqus.swig
│       │   ├── duoshuo.swig
│       │   ├── gitment.swig
│       │   ├── hypercomments.swig
│       │   ├── index.swig
│       │   ├── livere.swig
│       │   ├── valine.swig
│       │   └── youyan.swig
│       ├── duoshuo-hot-articles.swig
│       ├── exturl.swig
│       ├── mathjax.swig
│       ├── needsharebutton.swig
│       ├── rating.swig
│       ├── schedule.swig
│       ├── scroll-cookie.swig
│       ├── search
│       │   ├── algolia-search
│       │   ├── index.swig
│       │   ├── localsearch.swig
│       │   └── tinysou.swig
│       └── seo
│           └── baidu-push.swig
├── LICENSE
├── package.json
├── README.md
├── scripts
│   ├── merge-configs.js
│   ├── merge.js
│   └── tags
│       ├── button.js
│       ├── center-quote.js
│       ├── exturl.js
│       ├── full-image.js
│       ├── group-pictures.js
│       ├── label.js
│       ├── lazy-image.js
│       ├── note.js
│       └── tabs.js
├── source
│   ├── css
│   │   ├── _common
│   │   │   ├── components
│   │   │   │   ├── back-to-top-sidebar.styl
│   │   │   │   ├── back-to-top.styl
│   │   │   │   ├── buttons.styl                             # it has the least priority of post button
│   │   │   │   ├── comments.styl
│   │   │   │   ├── components.styl
│   │   │   │   ├── footer
│   │   │   │   │   └── footer.styl
│   │   │   │   ├── header
│   │   │   │   │   ├── headerband.styl
│   │   │   │   │   ├── header.styl
│   │   │   │   │   ├── menu.styl
│   │   │   │   │   ├── site-meta.styl
│   │   │   │   │   └── site-nav.styl
│   │   │   │   ├── highlight
│   │   │   │   │   ├── diff.styl
│   │   │   │   │   ├── highlight.styl
│   │   │   │   │   └── theme.styl
│   │   │   │   ├── pages
│   │   │   │   │   ├── archive.styl
│   │   │   │   │   ├── categories.styl
│   │   │   │   │   ├── pages.styl
│   │   │   │   │   ├── post-detail.styl
│   │   │   │   │   └── schedule.styl
│   │   │   │   ├── pagination.styl
│   │   │   │   ├── post
│   │   │   │   │   ├── post-button.styl                       # it has the second highest priority of post button
│   │   │   │   │   ├── post-collapse.styl
│   │   │   │   │   ├── post-copyright.styl
│   │   │   │   │   ├── post-eof.styl
│   │   │   │   │   ├── post-expand.styl
│   │   │   │   │   ├── post-gallery.styl
│   │   │   │   │   ├── post-meta.styl
│   │   │   │   │   ├── post-nav.styl
│   │   │   │   │   ├── post-reward.styl
│   │   │   │   │   ├── post-rtl.styl
│   │   │   │   │   ├── post.styl
│   │   │   │   │   ├── post-tags.styl
│   │   │   │   │   ├── post-title.styl
│   │   │   │   │   ├── post-type.styl
│   │   │   │   │   └── post-widgets.styl
│   │   │   │   ├── sidebar
│   │   │   │   │   ├── sidebar-author-links.styl
│   │   │   │   │   ├── sidebar-author.styl
│   │   │   │   │   ├── sidebar-blogroll.styl
│   │   │   │   │   ├── sidebar-dimmer.styl
│   │   │   │   │   ├── sidebar-feed-link.styl
│   │   │   │   │   ├── sidebar-nav.styl
│   │   │   │   │   ├── sidebar.styl                       # sidebar.styl modify sidebar style configuration
│   │   │   │   │   ├── sidebar-toc.styl
│   │   │   │   │   ├── sidebar-toggle.styl
│   │   │   │   │   └── site-state.styl
│   │   │   │   ├── tag-cloud.styl
│   │   │   │   ├── tags
│   │   │   │   │   ├── blockquote-center.styl
│   │   │   │   │   ├── exturl.styl
│   │   │   │   │   ├── full-image.styl
│   │   │   │   │   ├── group-pictures.styl
│   │   │   │   │   ├── label.styl
│   │   │   │   │   ├── note-modern.styl
│   │   │   │   │   ├── note.styl
│   │   │   │   │   ├── tabs.styl
│   │   │   │   │   └── tags.styl
│   │   │   │   └── third-party
│   │   │   │       ├── algolia-search.styl
│   │   │   │       ├── baidushare.styl
│   │   │   │       ├── busuanzi-counter.styl
│   │   │   │       ├── duoshuo.styl
│   │   │   │       ├── gitalk.styl
│   │   │   │       ├── gitment.styl
│   │   │   │       ├── han.styl
│   │   │   │       ├── jiathis.styl
│   │   │   │       ├── localsearch.styl
│   │   │   │       ├── mob_share.styl
│   │   │   │       └── needsharebutton.styl
│   │   ├── _custom
│   │   │   └── custom.styl                               # this custom.styl possess the highest priority that could overwite other button.style
│   │   ├── main.styl
│   │   ├── _mixins
│   │   │   ├── base.styl
│   │   │   ├── custom.styl
│   │   │   ├── Gemini.styl
│   │   │   ├── Mist.styl
│   │   │   ├── Muse.styl
│   │   │   └── Pisces.styl
│   │   ├── _schemes
│   │   │   ├── Gemini
│   │   │   ├── Mist
│   │   │   ├── Muse
│   │   │   └── Pisces
│   │   └── _variables
│   │       ├── base.styl
│   │       ├── custom.styl
│   │       ├── Gemini.styl
│   │       ├── Mist.styl
│   │       ├── Muse.styl
│   │       └── Pisces.styl
│   ├── fonts
│   ├── images
│   ├── js
│   │   └── src
│   │       ├── affix.js
│   │       ├── algolia-search.js
│   │       ├── bootstrap.js
│   │       ├── exturl.js
│   │       ├── hook-duoshuo.js
│   │       ├── js.cookie.js
│   │       ├── love.js
│   │       ├── motion.js
│   │       ├── post-details.js
│   │       ├── schemes
│   │       ├── scroll-cookie.js
│   │       ├── scrollspy.js
│   │       └── utils.js
│   └── lib
│       ├── algolia-instant-search
│       ├── canvas-nest
│       ├── canvas-ribbon
│       ├── fancybox
│       ├── fastclick
│       ├── font-awesome
│       ├── Han
│       ├── jquery
│       ├── jquery_lazyload
│       ├── needsharebutton
│       ├── pace
│       ├── three
│       ├── ua-parser-js
│       └── velocity
└── test
    ├── helpers.js
    └── intern.js
```
