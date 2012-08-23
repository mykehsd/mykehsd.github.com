---
layout: default
title: #Magento merged css/js file (CDN & Browser) cache busting.
---
<p>A client that uses magento on a CDN was having problems with releasing new code (specifically to CSS and JS files). &nbsp;When magento uses merged files it keeps the merged filename the same regardless of the contents of the file or release of code. &nbsp;This is especially nasty if you have a long TTL cache on client browsers. &nbsp;</p>
<p>In our deployment process we maintain a release.txt file to ensure all of the frontend servers are running the exact release. &nbsp;We are going to use that file to increment our merged css/js files - however you could use something like a md5 checksum of the contents and append it to the file name. &nbsp;</p>
<p>Anyway, here's our solution:</p>
<p><iframe width="700" height="400" src="http://pastebin.com/embed_iframe.php?i=L7snsHLd"></iframe></p>
<p>So as soon as a release is executed in our production environment, it immediately invalidates both the CDN and client side cache. &nbsp;We can also set a 365D TTL for client's browers to cache all CSS and JS since the filenames will change when an update happens. &nbsp;</p>
