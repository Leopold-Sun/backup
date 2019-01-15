---
title: Baidu-Google SEO
date: 2018-12-26 13:11:10
tags: [Hexo]
categories: Hexo
description:
keywords: hexo baidu seo, hexo google seo
---

1. SEO with [Google Search Console](https://search.google.com/search-console?resource_id=https%3A%2F%2Fleopold-sun.com%2F)
	- Add resource: add your defined url such as <leopold-sun.com>
		- ![add-resource](/images/hexo-seo/add-resource.png)	
	- Google verfication
		- ![google-site-verification](/images/hexo-seo/google-site-verification.png)
	- Modify site -config.yml residing in your blog root directory
		- add the following configuration
			```
			sitemap:
				path: sitemap.xml
			```
	- Add sitemap.xml to google search console by inputting url of the sitemap.xml such as `https://leopold-sun.com/sitemap.xml`
		- Before that, it is nacessary to generate public files using `hexo g` and `hexo d` to deploying those public files to github such that the sitemap.xml could be accessed by google search console.
		- ![add-new-sitemap](/images/hexo-seo/add-new-sitemap.png)

<!-- more  -->

2. SEO with [Baidu-Site](https://ziyuan.baidu.com/dashboard/index?site=https://leopold-sun.com/)
	- Sign up if needed
	- Add site url: such as
		`leopold-sun.com`
		![baidu-add-resource](/images/hexo-seo/baidu-add-resource.png)
	- Account verification 
		- Similar to google site verification
	- Modify site -config.yml residing in your blog root directory
		- add the following configuration
			```
			baidusitemap:
				path: baidusitemap.xml
			```

3. Pusing to Baidu
Location: ![link-push](/images/hexo-seo/link-push.png)
Push choices: ![push-alters](/images/hexo-seo/push-alters.png)
- Active push
	- install submit extension

	```
	npm install hexo-baidu-url-submit --save   
	```

	- Modify site -config.yml

	```
	baidu_url_submit:
  	count: 5 
  	host: leopold-sun.com ## site url
  	token: your_token ## token
  	path: baidu_urls.txt
	```

	- add new type to deployer in site -config.yml

	```
	deploy:
	- type: git
  	repo:
  	branch:
	- type: baidu_url_submitter
	```

	- Pushing options

	```
	hexo g            ## output the baidu_urls.txt
	hexo d            ## read urls from baidu_urls.txt and push to baidu site
	```

- Auto push
	- Copy js script from [Baidu Search Engine](https://ziyuan.baidu.com/)
	![auto-push](/images/hexo-seo/auto-push.png)

	```
	<script>
	(function(){
    	var bp = document.createElement('script');
    	var curProtocol = window.location.protocol.split(':')[0];
    	if (curProtocol === 'https') {
        	bp.src = 'https://zz.bdstatic.com/linksubmit/push.js';
    	}
    	else {
        	bp.src = 'http://push.zhanzhang.baidu.com/push.js';
    	}
    	var s = document.getElementsByTagName("script")[0];
    	s.parentNode.insertBefore(bp, s);
	})();
	</script>
	```
- Sitemap
	- push the baidusitemap.xim which resides in `public` after `hexo generate`

4. Test SEO of Google and Baidu
Test using `site: leopold-sun.com`
