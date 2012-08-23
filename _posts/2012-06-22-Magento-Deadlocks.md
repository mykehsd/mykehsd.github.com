---
layout: default
title: Magento Deadlocks
tags: 
- magento
- deadlocks
---
<p>If your using memcache as your caching layer in magento, but have this snippit in your etc/local.xml</p>
<blockquote>
<p>&nbsp;</p>
<p>&nbsp;&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;&lt;slow_backend&gt;database&lt;/slow_backend&gt;</p>
<p>&nbsp;&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;&lt;slow_backend_store_data&gt;1&lt;/slow_backend_store_data&gt;</p>
<p>&nbsp;&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;&lt;auto_refresh_fast_cache&gt;1&lt;/auto_refresh_fast_cache&gt;&nbsp;</p>
<p>&nbsp;</p>
</blockquote>
<p>&nbsp;</p>
<p>then your in for trouble. &nbsp;First off, this is a magento recommend setting for all 1.9 Enterprise customers. &nbsp;If you use this setting with even a moderate level of traffic hitting your site ~don't~ make any changes via the admin on your catalog products. &nbsp;It'll cause a deadlock situation and bring down your site for 5-15 minutes. &nbsp;We are still experimenting with a solution, but there are two possible ones:</p>
<p><span style="white-space: pre;"> </span>-segregate your Admin traffic to an isolated server. &nbsp;On this server remove all of the slow_backend config settings in your etc/local.xml</p>
<p><span style="white-space: pre;"> </span>-it seems that turning&nbsp;slow_backend_store_data off ~may~ solve it. &nbsp;</p>
<p>&nbsp;</p>
<p>Your not getting deadlocks you say? &nbsp;Make sure you have your errors/local.xml setup to email you when you have a platform exception, you'll start noticing all of the lock timeouts and deadlocks.</p>
<p>&nbsp;</p>
