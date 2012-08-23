---
layout: default
title: Magento/GoogleCheckout and Disabled products
---
<p>If you have a high rate of orders, or customers that take their time finalizing checkout, then you might have noticed an odd problem with your Sales &gt; Orders. &nbsp;If a customer adds product XYZ into their cart and proceeds to Google Checkout, then a product is deactivated from the magento system, we have no way of stopping the order flow for that customer - At least not a reliable one. &nbsp;What happens is an order created in the magento system with no products and $0 due. &nbsp;The order looks totally find for the customer however you'll never be able to cancel the order based on the canCancel() method in Mage_Sales_Model_Order. &nbsp;</p>
<p>The solution is to watch the &lt;checkout_submit_all_after&gt; event and immediately cancel orders as they are created. &nbsp;You could override the&nbsp;Mage_GoogleCheckout_Model_Api_Xml_Callback::_responseNewOrderNotification() method, however I tried to be as non-obtrusive as I can with magento core code. &nbsp;</p>
<p>&nbsp;</p>
<p>YourCustomModule/GoogleCheckout/etc/config.xml</p>
<blockquote>
<p>&nbsp;&nbsp; &nbsp; &nbsp; &nbsp;&lt;events&gt;</p>
<p>&nbsp;&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;&lt;checkout_submit_all_after&gt;</p>
<p>&nbsp;&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;&lt;observers&gt;</p>
<p>&nbsp;&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;&lt;yourcustommodule_googlecheckout_observer&gt;</p>
<p>&nbsp;&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;&lt;type&gt;singleton&lt;/type&gt;</p>
<p>&nbsp;&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;&lt;class&gt;YourModule_GoogleCheckout_Model_Observer&lt;/class&gt;</p>
<p>&nbsp;&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;&lt;method&gt;observeForEmptyOrders&lt;/method&gt;</p>
<p>&nbsp;&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;&lt;yourcustommodule_googlecheckout_observer&gt;</p>
<p>&nbsp;&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;&lt;/observers&gt;</p>
<p>&nbsp;&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;&lt;/checkout_submit_all_after&gt;</p>
<p>&nbsp;&nbsp; &nbsp; &nbsp; &nbsp;&lt;/events&gt;</p>
</blockquote>
<p>&nbsp;</p>
<p>And in your Model/Observer.php class:</p>
<blockquote>
<p>class FoxConnect_GoogleCheckout_Model_Observer</p>
<p>{</p>
<p>&nbsp;&nbsp; &nbsp;public function observeForEmptyOrders($observer) {</p>
<p>&nbsp;&nbsp; &nbsp; &nbsp; &nbsp;$order = $observer-&gt;getEvent()-&gt;getOrder();</p>
<p>&nbsp;&nbsp; &nbsp; &nbsp; &nbsp;if ($order-&gt;getPayment()-&gt;getMethod() != 'googlecheckout') &nbsp;return;</p>
<p>&nbsp;&nbsp; &nbsp; &nbsp; &nbsp;if (count($order-&gt;getAllItems()) == 0) {</p>
<p>&nbsp;&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;$order-&gt;getPayment()-&gt;getMethodInstance()-&gt;cancel( $order-&gt;getPayment());</p>
<p>&nbsp;&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;$order-&gt;setStatus( Mage_Sales_Model_Order::STATE_CANCELED );</p>
<p>&nbsp;&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;$order-&gt;save();</p>
<p>&nbsp;&nbsp; &nbsp; &nbsp; &nbsp;}</p>
<p>&nbsp;&nbsp; &nbsp;}</p>
<p>}</p>
</blockquote>
