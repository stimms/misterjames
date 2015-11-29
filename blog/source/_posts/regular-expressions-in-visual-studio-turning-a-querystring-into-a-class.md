title: Regular Expressions in Visual Studio - Turning A Querystring into a Class
tags:
  - Asp.Net MVC
  - PayPal
  - Visual Studio 11
  - Visual Studio 2010
id: 721
categories:
  - Code Dive
date: 2012-05-22 17:51:00
---

> This post will help you create a class that can be used in MVC Framework model binding, but is also a good demonstration of the Regular Expression support in Visual Studio's Find &amp; Replace feature.&nbsp; The class created can be used with PayPal's IPN notifications. 

I just started working with the PayPal "Instant Payment Notification" API again. I wanted to use the model binding capabilities of Asp.Net MVC, so the first task was to extract the querystring variables and make properties out of them.&nbsp; The sample querystring from the PayPay documentation for IPN is as follows:
<pre class="csharpcode" style="margin-left: 15px">mc_gross=19.95&amp;protection_eligibility=Eligible&amp;address_status=confirmed&amp;payer_id=LPLWNMTBWMFAY
&amp;tax=0.00&amp;address_street=1+Main+St&amp;payment_date=20%3A12%3A59+Jan+13%2C+2009+PST&amp;payment_status=Completed
&amp;charset=windows-1252&amp;address_zip=95131&amp;first_name=Test&amp;mc_fee=0.88&amp;address_country_code=US&amp;address_name=Test+User
&amp;notify_version=2.6&amp;custom=&amp;payer_status=verified&amp;address_country=United+States&amp;address_city=San+Jose
&amp;quantity=1&amp;verify_sign=AtkOfCXbDm2hu0ZELryHFjY-Vb7PAUvS6nMXgysbElEn9v-1XcmSoGtf&amp;payer_email=gpmac_1231902590_per%40paypal.com
&amp;txn_id=61E67681CH3238416&amp;payment_type=instant&amp;last_name=User&amp;address_state=CA&amp;receiver_email=gpmac_1231902686_biz%40paypal.com
&amp;payment_fee=0.88&amp;receiver_id=S8XGHLYDW9T3S&amp;txn_type=express_checkout&amp;item_name=&amp;mc_currency=USD&amp;item_number=
&amp;residence_country=US&amp;test_ipn=1&amp;handling_amount=0.00&amp;transaction_subject=&amp;payment_gross=19.95&amp;shipping=0.00</pre>

As you can see, it's a bit of a mess.&nbsp; It's all on one line (if you copy and paste their sample) and you will need to break out the key-value pairs. This is made especially easy in Visual Studio, if you know a few tricks.

Start by adding a blank Text Document to your project and then paste in the IPN querystring provided by PayPal.&nbsp; Here are the steps to build the class:

1.  Press CTRL + H to invoke the replace dialog. The first replace operation is to search for &amp; and to replace with \n. Make sure you have "Use Regular Expressions" enabled in the options pull down.&nbsp; We're simply searching for an ampersand character and replacing it with a new line.
![image](http://oldblog.jameschambers.com/Media/Default/Windows-Live-Writer/4181caf209ff_AB4E/image_5.png "image")<li>Press the button for Replace All and it'll break everything on to separate lines for you.<li>Next, search for =.* and replace it with nothing. This blanks out everything from the = sign on.<li>Finally, we need to capture the text on the entire line. I had a couple of hiccups trying to get the capture working in Visual Studio, but this worked for me:
    1.  Search for: ^{.*}$<li>Replace with: public string \1 { get; set; }
![image](http://oldblog.jameschambers.com/Media/Default/Windows-Live-Writer/4181caf209ff_AB4E/image_9.png "image")

&nbsp;

^ is a character for "start of line" and $ is the character representing "end of line". The {.*} bits in between captures everything else on the line into a numbered group. Captured groups are numbered and start at 1.&nbsp; In our replace statement, we're basically writing out properties and using the \1 to insert the property name that we captured in the curly braces. 

> Note: at the time of this writing, I had to use VS2010 to do this last operation because VS11 Beta is not really giving the regex a lot of love.

When you've completed all those bits, you'll have a bunch of properties ready to paste into a class:

![image](http://oldblog.jameschambers.com/Media/Default/Windows-Live-Writer/4181caf209ff_AB4E/image_12.png "image")

If you're interested, this is the class you need as an action parameter in an Asp.Net MVC project to capture the querystring parameters provided by the PayPal Instant Payment Notification (or IPN):
<pre class="csharpcode">    <span class="kwrd">public</span> <span class="kwrd">class</span> PayPalIpnRequest
    {
        <span class="kwrd">public</span> <span class="kwrd">string</span> mc_gross { get; set; }
        <span class="kwrd">public</span> <span class="kwrd">string</span> protection_eligibility { get; set; }
        <span class="kwrd">public</span> <span class="kwrd">string</span> address_status { get; set; }
        <span class="kwrd">public</span> <span class="kwrd">string</span> payer_id { get; set; }
        <span class="kwrd">public</span> <span class="kwrd">string</span> tax { get; set; }
        <span class="kwrd">public</span> <span class="kwrd">string</span> address_street { get; set; }
        <span class="kwrd">public</span> <span class="kwrd">string</span> payment_date { get; set; }
        <span class="kwrd">public</span> <span class="kwrd">string</span> payment_status { get; set; }
        <span class="kwrd">public</span> <span class="kwrd">string</span> address_zip { get; set; }
        <span class="kwrd">public</span> <span class="kwrd">string</span> first_name { get; set; }
        <span class="kwrd">public</span> <span class="kwrd">string</span> mc_fee { get; set; }
        <span class="kwrd">public</span> <span class="kwrd">string</span> address_country_code { get; set; }
        <span class="kwrd">public</span> <span class="kwrd">string</span> address_name { get; set; }
        <span class="kwrd">public</span> <span class="kwrd">string</span> notify_version { get; set; }
        <span class="kwrd">public</span> <span class="kwrd">string</span> custom { get; set; }
        <span class="kwrd">public</span> <span class="kwrd">string</span> payer_status { get; set; }
        <span class="kwrd">public</span> <span class="kwrd">string</span> address_country { get; set; }
        <span class="kwrd">public</span> <span class="kwrd">string</span> address_city { get; set; }
        <span class="kwrd">public</span> <span class="kwrd">string</span> quantity { get; set; }
        <span class="kwrd">public</span> <span class="kwrd">string</span> verify_sign { get; set; }
        <span class="kwrd">public</span> <span class="kwrd">string</span> payer_email { get; set; }
        <span class="kwrd">public</span> <span class="kwrd">string</span> txn_id { get; set; }
        <span class="kwrd">public</span> <span class="kwrd">string</span> payment_type { get; set; }
        <span class="kwrd">public</span> <span class="kwrd">string</span> last_name { get; set; }
        <span class="kwrd">public</span> <span class="kwrd">string</span> address_state { get; set; }
        <span class="kwrd">public</span> <span class="kwrd">string</span> receiver_email { get; set; }
        <span class="kwrd">public</span> <span class="kwrd">string</span> payment_fee { get; set; }
        <span class="kwrd">public</span> <span class="kwrd">string</span> receiver_id { get; set; }
        <span class="kwrd">public</span> <span class="kwrd">string</span> txn_type { get; set; }
        <span class="kwrd">public</span> <span class="kwrd">string</span> item_name { get; set; }
        <span class="kwrd">public</span> <span class="kwrd">string</span> mc_currency { get; set; }
        <span class="kwrd">public</span> <span class="kwrd">string</span> item_number { get; set; }
        <span class="kwrd">public</span> <span class="kwrd">string</span> residence_country { get; set; }
        <span class="kwrd">public</span> <span class="kwrd">string</span> test_ipn { get; set; }
        <span class="kwrd">public</span> <span class="kwrd">string</span> handling_amount { get; set; }
        <span class="kwrd">public</span> <span class="kwrd">string</span> transaction_subject { get; set; }
        <span class="kwrd">public</span> <span class="kwrd">string</span> payment_gross { get; set; }
        <span class="kwrd">public</span> <span class="kwrd">string</span> shipping { get; set; }

    }</pre>
<style type="text/css">.csharpcode, .csharpcode pre
{
	font-size: small;
	color: black;
	font-family: consolas, "Courier New", courier, monospace;
	background-color: #ffffff;
	/*white-space: pre;*/
}
.csharpcode pre { margin: 0em; }
.csharpcode .rem { color: #008000; }
.csharpcode .kwrd { color: #0000ff; }
.csharpcode .str { color: #006080; }
.csharpcode .op { color: #0000c0; }
.csharpcode .preproc { color: #cc6633; }
.csharpcode .asp { background-color: #ffff00; }
.csharpcode .html { color: #800000; }
.csharpcode .attr { color: #ff0000; }
.csharpcode .alt 
{
	background-color: #f4f4f4;
	width: 100%;
	margin: 0em;
}
.csharpcode .lnum { color: #606060; }
</style>