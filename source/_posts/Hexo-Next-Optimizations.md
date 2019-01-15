---
title: Hexo+Next+Optimizations
date: 2018-12-23 13:34:32
tags: Hexo
categories: Hexo
description: use these optimizations to modify your own blog
---
### Debug with Google developer tools
Ubuntu 18.04
```
Ctrl+Shift+I
```

### Display next page categories and tags in menu
1. creat page
```
hexo new page categories 
hexo new page tags
hexo new page about
```

<!-- more -->

2. configure menu
location: `/themes/next/_config.yml`
```
menu:
  home: / || home
  #about: /about/ || user
  #tags: /tags/ || tags
  #categories: /categories/ || th
  archives: /archives/ || archive
  categories: /categories/ || th
  tags: /tags/ || tags
  Leopold-Seagal: /Leopold-Seagal/ || user
```
3. modify index.md of tags
Location: `/source/tags/index.md`
```
title: tags
date: 2018-12-21 21:29:26
type: "tags"
layout: "tags"
```

4. modify index.md of categories
Location: `/source/categories/index.md`
```
title: categories
date: 2018-12-21 23:33:50
type: "categories"
layout: "categories"
```
-----------------
### Show headshot in circle manner and rotate it
Location: `themes/next/source/css/_common/components/sidebar/sidebar.styl`
```
.site-author-image {
  display: block;
  margin: 0 auto;
  padding: $site-author-image-padding;
  max-width: $site-author-image-width;
  height: $site-author-image-height;
  border: site-author-image-border-color;
  /* start*/
  border-radius: 50%
  webkit-transition: 1.4s all;
  moz-transition: 1.4s all;
  ms-transition: 1.4s all;
  transition: 1.4s all;
  /* end */
}
/* start */
.site-author-image:hover {
  background-color: #55DAE1;
  webkit-transform: rotate(360deg) scale(1.1);
  moz-transform: rotate(360deg) scale(1.1);
  ms-transform: rotate(360deg) scale(1.1);
  transform: rotate(360deg) scale(1.1);
}
/* end */
```
------------------------
### Show wordcounts in post header
1. Install wordcount
```
npm install hexo-wordcount@2 --save
```
 

2. Modift config.yml of next theme
Location: `/theme/next/_config.yml`

```
post_wordcount:
  item_text: true
  wordcount: true
  min2read: false
  totalcount: false
  separated_meta: false
```
-------------------
### Personal custom.styl

- used links
> [ookamiAntD](https://yangbingdong.com/)
> [Yang ZiHao](http://www.cduyzh.com/hexo-settings-3/)
> [reuixiy](https://reuixiy.github.io/technology/computer/computer-aided-art/2017/06/09/hexo-next-optimization.html)
- My custom.styl
Location: `themes/next/source/css/_custom/custom.styl`

```
//background

//body
body {
    background: url(/images/mobilepaper/black3.jpeg);
    background-attachment: fixed;
    background-size: cover;
}


///////////////////////////////////////////////////////////////////////////

.headband {
    height: 0px;
    background-image: linear-gradient(90deg, #F79533 0%, #F37055 15%, #EF4E7B 30%, #A166AB 44%, #5073B8 58%, #1098AD 72%, #07B39B 86%, #6DBA82 100%);
}

.header {
    line-height: 1; 
    background: url(/images/black.jpeg);
}


.header-inner {
    padding-top: 5px;
    padding-bottom: 0px;
}
.posts-expand {
    padding-top: 80px;
    background-color: #f9f9f966;
}
.posts-expand .post-meta {
    margin: 5px 0px 0px 0px;
}

.container .header-inner {
    width: 100%;
}

.brand {
    margin-top: 15px
    padding: 0;
    background-color: rgba(255, 255, 255, 0);
}

/////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////

//site configuration
.brand{
    background-color: rgba(255, 255, 255, 0);
    margin-top: 15px;
    padding: 0px;
}
.site-title {
    font-size: 45px;
    font-weight: initial;
    color: #fff;
    line-height: 60px;
    letter-spacing: 60px;
    border-top: dashed;
    border-bottom: dashed;
    border-top-width: 2px;
    border-bottom-width: 2px;
    padding-left: 50px;
}
.site-subtitle {
    margin: 0px;
    font-size: 18px;
    letter-spacing: 14px;
    padding-bottom: 10px;
    padding-top: 10px;
    color: #fff;
    font-weight: initial;
    font-family: Comic Sans MS;
}

////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////

//menu
.menu {
    text-align: center;
    margin: 0 0 0 0;
    padding: 5px;
    box-shadow: 0px 10px 10px 0px rgba(0, 0, 0, 0.74);
    margin-left: auto;
    margin-right: auto;
}
.menu .menu-item a {
    display: block;
    font-size: 14px;
    line-height: inherit;
    transition-property: border-color;
    transition-duration: 0.2s;
    transition-timing-function: ease-in-out;
    transition-delay: 0s;
    color: white;
    text-decoration: none;
    outline: #f9f9f9;
    background-color: transparent;
}
.menu .menu-item {
    margin: 5px 15px;
}
.menu .menu-item a:hover {
    border-bottom-color: rgba(161, 102, 171, 0);
}

//////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////
.post {
    margin-bottom: 10px;
    padding: 10px 36px 10px 36px;
    box-shadow: 0 0 10px 0 rgba(0,0,0,.5);
    background-color: #fff;
}
.posts-expand .post-title {
    font-size: 26px;
    font-weight: 700;
}

.posts-expand .post-tags a {
    display: inline-block;
    margin-right: 10px;
    font-size: 16px;
    font-weight: bold;
    color: aqua;
}

.posts-expand .post-title-link::before {
    background: url(/images/black.jpeg);
}
.pagination {
    border: none;
    margin: 0px;
}
.post-nav-item a {
    color: #fff;
    font-weight: bold;
}
.post-nav-item a:hover {
    color: rgb(161, 102, 171);
    font-weight: bold;
}

.posts-expand .post-body img {
    border: none;
    padding: 0px;
}
.post-gallery .post-gallery-img img {
    padding: 3px;
}
.sidebar-toggle {
    right: 10px;
    bottom: 43px;
    background-color: rgba(247, 149, 51, 0.75);
    border-radius: 5px;
    box-shadow: 0px 0px 10px 0px rgba(0, 0, 0, 0.35);
}
.page-post-detail .sidebar-toggle-line {
    background: rgb(7, 179, 155);
}

//modify post page size
.main-inner {
    width: 1024px;
}

//archive page
.posts-collapse .collection-title h1, .posts-collapse .collection-title h2 {
    color: #5CB885;
}

////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////

//back to top

.back-to-top.back-to-top-on {
    bottom: 10px;
    display: block;
}

.back-to-top {
    color: chartreuse;
    line-height: 2;
    right: 10px;
    padding-right: 5px;
    padding-left: 5px;
    padding-top: 2.5px;
    padding-bottom: 2.5px;
    background-color: #222;
    border-radius: 5px;
    box-shadow: 0px 0px 10px 0px rgba(0,0,0,0.35);
}


////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////

//footer
.footer {
    line-height: 1;
    background-color: #f9f9f982;
    color: #222;
}

.pagination .page-number.current {
    border-radius: 100%;
    color: chartreuse;
    box-shadow: 0px 0px 10px 0px rgba(0,0,0,0.5);
    background-color: #222;
}

.pagination .page-number, .pagination .space {
    display: inline-block;
    color: #5cb85c;
    position: relative;
    top: -1px;
    margin: 0 10px;
    padding: 0 11px;
}

//Leopold-Seagal
.fa-tint:before {
    content: "\f043";
    color: black;
}

.post-tags {
    background: url(/images/black.jpeg);
    background-attachment: contain;
}


/////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////

.sidebar a {
    color: #55dae1;
}

//display links' border-bottom
.links-of-blogroll-item a {
    color: #5fd2cd96;
    border-bottom: none;
}

.sidebar-inner {
    position: relative;
    padding: 20px 10px;
    color: #55dae1;
    text-align: center;
    background: url(/images/black.jpeg);
}

.sidebar-toggle {
    right: 10px;
    bottom: 43px;
    background-color: rgba(247,149,51,.75);
    border-radius: 5px;
    box-shadow: 0 0 10px 0 rgba(0,0,0,.35);
    position: fixed;
    width: 14px;
    height: 14px;
    padding: 5px;
    background: #222;
    line-height: 0;
    z-index: 1050;
    cursor: pointer;
}

.site-author-image {
    display: block;
    margin: 10px auto;
}


///////////////////////////////////////////////////////////////////////////////////////////////////////////
// mobile configuration

@media (max-width: 1023px) {
    .container {
        background-color: rgba(0, 0, 0, 0);
    }
    .sidebar {
        box-shadow: 0px 0px 10px 0px rgba(0, 0, 0, 0.5);
        border-top-left-radius: 5px;
        border-bottom-left-radius: 5px;
    }
    .feed-link {
        display: none !important;
    }
}
@media (max-width: 767px) {
    .main {
        padding-bottom: 120px;
	background: url(/images/mobilepaper/black2.jpeg);
	background-attachment: fixed;
	background-size: cover;
    }
    .posts-expand {
        margin: 0px;
        padding-top: 70px;
    }
    .posts-expand .post-title {
        font-size: 22px;
    }
    .posts-expand .post-meta {
        font-size: 10px;
    }
    .post {
        margin-bottom: 30px !important;
        padding: 20px 15px 15px 15px !important;
    }
    .post-body h2, h3, h4, h5, h6 {
        margin-left: -15px;
        padding-left: 11px;
    }
    .posts-expand .post-tags {
        margin-top: 10px;
    }
    .post-widgets {
        margin-top: 10px;
    }
    .comments {
        margin: 40px 0px 40px 0px !important;
    }
    .footer {
        box-shadow: 0px -5px 10px 0px rgba(0, 0, 0, 0.5);
    }
}
.sidebar-active #sidebar-dimmer {
    opacity: 0;
}
.site-nav-toggle {
    top: 35px;
    left: 10px;
}
.btn-bar {
    background-color: rgb(255, 255, 255);
}
@media (max-width: 767px) {
    .menu {
        text-align: center;
        box-shadow: 0px 5px 10px 0px rgba(0, 0, 0, 0.5);
    }
    .site-nav {
        top: initial;
        background-color: rgba(255, 255, 255, 0.75);
        border-top: none;
        border-bottom: none;
        position: relative;
        z-index: 1024;
    }
}
@media (max-width: 767px) {
    .site-title {
        font-size: 70px !important;
        letter-spacing: 0px !important;
    }
    .site-subtitle{
        letter-spacing: 0px !important;
        padding-bottom: 0px !important;
    }
    .site-meta {
        box-shadow: 0px 5px 10px 0px rgba(0, 0, 0, 0.5);
    }
    .menu .menu-item {
        margin: 0px 10px !important;
    }
}
```

---------------

### Clip to show love shape
- creat one js function
Location: `theme/next/source/js/src/love.js`
```
!function(e,t,a){function r(){for(var e=0;e<n.length;e++)n[e].alpha<=0?(t.body.removeChild(n[e].el),n.splice(e,1)):(n[e].y--,n[e].scale+=.004,n[e].alpha-=.013,n[e].el.style.cssText="left:"+n[e].x+"px;top:"+n[e].y+"px;opacity:"+n[e].alpha+";transform:scale("+n[e].scale+","+n[e].scale+") rotate(45deg);background:"+n[e].color+";z-index:99999");requestAnimationFrame(r)}var n=[];e.requestAnimationFrame=e.requestAnimationFrame||e.webkitRequestAnimationFrame||e.mozRequestAnimationFrame||e.oRequestAnimationFrame||e.msRequestAnimationFrame||function(e){setTimeout(e,1e3/60)},function(e){var a=t.createElement("style");a.type="text/css";try{a.appendChild(t.createTextNode(e))}catch(t){a.styleSheet.cssText=e}t.getElementsByTagName("head")[0].appendChild(a)}(".heart{width: 10px;height: 10px;position: fixed;background: #f00;transform: rotate(45deg);-webkit-transform: rotate(45deg);-moz-transform: rotate(45deg);}.heart:after,.heart:before{content: '';width: inherit;height: inherit;background: inherit;border-radius: 50%;-webkit-border-radius: 50%;-moz-border-radius: 50%;position: fixed;}.heart:after{top: -5px;}.heart:before{left: -5px;}"),function(){var a="function"==typeof e.onclick&&e.onclick;e.onclick=function(e){a&&a(),function(e){var a=t.createElement("div");a.className="heart",n.push({el:a,x:e.clientX-5,y:e.clientY-5,scale:1,alpha:1,color:"rgb("+~~(255*Math.random())+","+~~(255*Math.random())+","+~~(255*Math.random())+")"}),t.body.appendChild(a)}(e)}}(),r()}(window,document);
```
- modify layout.swig file to enable it
Location: `/themes/next/layout/layout.swig`
```
<script type="text/javascript" src="/js/src/love.js"></script>
```

--------------------

### Switch the "search" style from "block" to "none"

- Sytle = "display=block;"

	![page-search-block](/images/hexo-next-opt/display-block.png)

	- Google developer tools to find configuration
	![css-block](/images/hexo-next-opt/display-block-css.png)

- Sytle = "display=none;"
	
	![page-search-none](/images/hexo-next-opt/display-none-css.png)
	
	- Modify localsearch.swig residing in `/layout/_partials/search/`
	![search-swig](/images/hexo-next-opt/display-none-swig.png)

-------------------------

### Add live2d

1. Overview
![overview-live2d](/images/hexo-next-opt/overview-live2d.png)

2. Git clone packages
	- Git clone
	```
	git init
	git clone https://github.com/xiazeyu/live2d-widget-models.git
	```
	- Move to /node_modules
	![node_live2d_pack](/images/hexo-next-opt/node_live2d_pack.png)

3. Modify hexo -conifg.yml

```
live2d:
  enable: true 
  pluginModelPath: assets/
  model:
    use: live2d-widget-model-miku # 初音
#    use: live2d-widget-model-chitose  # 西装男青年
#    use: live2d-widget-model-epsilon2_1  # 粉发绿衣小迷妹
#    use: live2d-widget-model-haruto  # 小正太
#    use: live2d-widget-model-hibiki  # 棕发御姐
#    use: live2d-widget-model-hijiki  # 黑毛猫
#    use: live2d-widget-model-koharu  # 小萌妹
#    use: live2d-widget-model-shizuku  # 红棕课桌妹
#    use: live2d-widget-model-tororo  # 白毛猫
#    use: live2d-widget-model-unitychan  # 黄发双马尾小太妹
#    use: live2d-widget-model-z16  # 校园白衣女生
  display:
    position: left 
    width: 150 
    height: 345 
  mobile:
    show: false 
```

-------------------

### Modify post button such as "Read me"

1. List based on priority

> `custom.styl` > `post-button.styl` > `buttons.styl`

2. Locations

	- custom.styl: `themes/next/source/css/_custom/custom.styl`
	- post-button.styl: `themes/next/source/css/_common/components/post/post-button.styl`
	- buttons.styl: `themes/next/source/css/_common/components/buttons.styl`
3. Guess (could use other css to verify)
> The `custom.styl` always has the highest priority.
> The one with deepest path has higher priority.
