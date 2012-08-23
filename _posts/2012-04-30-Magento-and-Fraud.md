---
layout: default
title: Magento and Fraud prevention
tags: <i class="icon-tag"></i>magento <i class="icon-tag"></i>fraud <i class="icon-tag"></i>ecommerce
---
<p>There are many companies and products to help us stop fraudulent orders from entering systems. &nbsp;Some are good, some aren't, all are expensive. &nbsp;As a result many, many ecommerce shops create their own prevention mechanisms. &nbsp;</p>
<p>One that I've seen fairly effective (especially with international sites) is device fingerprinting. &nbsp;The theory is that your IDS system tags every user entering the system with a unique key that identifies a particular device. &nbsp;Once a user is flagged as fraudulent, that unique key can be used in the future to automatically flag new orders - even if the user registered new acccounts to place the order. &nbsp;</p>
<p>If your running a multi-site system, this could also be utilized across all of them to give you a higher level of detection (power in numbers). &nbsp;We recently implemented a <a href="https://github.com/carlo/jquery-browser-fingerprint">jQuery plugin version</a> of this device fingerprinting that has given surprising results. &nbsp;This is by no means a catch all, serious level of protection but it's definately going to stop your amature con-artist. &nbsp;</p>
<p>&nbsp;</p>
