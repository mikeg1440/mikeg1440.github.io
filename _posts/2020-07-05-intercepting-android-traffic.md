---
layout: post
title: Intercepting Android Traffic
date: 2020-07-05 23:17 -0400
---
<div class='text-center'>
  <img src="https://desk.zoho.com/DocsDisplay?zgId=687723208&mode=inline&blockId=dshu95d32e80c5490485fac29ad265118cc1b" alt='Burp and Android banner' />
</div>

Being interested in Cyber Security I'm well aware of the benefit of being able to intercept of Man-In-The-Middle a device to see exactly who its communicating with and sometimes what it's saying.  Having switched over to Android recently and buying a Chinese phone, a Xiomi Mi 8, I was curious to see the traffic that apps where sending.  There are a few tools out there that give you the ability to intercept traffic but the one I will be using is the [Community Burp Suite Edition](https://portswigger.net/burp/communitydownload).  This post will look at the set up to view intercept and modify the traffic of Android devices.  

It should be noted this is being for personal research on my own devices and own network.  You should never do this to any device you don't own without permission first.

<div class='text-center'>
  <img src="{{ site.baseurl }}/assets/img/burp/Burp_Splash.png" alt='Burp Suite Splash Screen' />
</div>

First thing we need to do is fire up Burp Suite.  If your not familiar with Burp then when you see the first prompts in the beginning just hit `Ok` then `Temporary Project` unless you want to save the project to disk then select that option.

<div class='text-center'>
  <img src="{{ site.baseurl }}/assets/img/burp/Burp_Project.png" alt='Burp project screenshot' />
</div>

Now we have the option of using a customized configuration, it's fine to select `Use Burp Defaults` and then click `Start Burp`.

<div class='text-center'>
  <img src="{{ site.baseurl }}/assets/img/burp/Burp_Settings.png" alt='Burp config options screenshot' />
</div>

The next part we have to do is set the listener for Burp.  We do this by clicking on the `Proxy` tab at the top then select the `Options` sub tab that pops up.  There it shows the listeners set for Burp, right now its listening on the localhost but I want to be able to connect through my wifi with my phone.  

<div class='text-center'>
  <img src="{{ site.baseurl }}/assets/img/burp/Burp_Listener.png" alt='Burp proxy settings' />
</div>

So we have to click `Edit` on the listener and select `Any Interface` option for the `Bind to address` setting.

<div class='text-center'>
  <img src="{{ site.baseurl }}/assets/img/burp/Burp_Listener_Select.png" alt='Listener edit screen' />
</div>

And now you should see a `*` where localhost was before.

<div class='text-center'>
  <img src="{{ site.baseurl }}/assets/img/burp/Burp_Listener_All.png" alt='Burps proxy settings screen' />
</div>

That's all we need to do get the intercept proxy up and running.  Now we need to connect to the proxy with the android device.  We do that by going to the phone settings page and then the wifi section.  When you see the wifi network your connected to select the options or more details for that network.  You should see a setting that says `Proxy`, go ahead and enable that and set it to manual then enter the IP address of the machine running Burp (just run `ifconfig` on linux).  Make sure you set the port to `8080` as well then apply the new settings.  

<div class='text-center'>
  <img src="{{ site.baseurl }}/assets/img/burp/Network_Details.png" alt='Android network settings screenshot' />
</div>


We should be connected to the proxy now but if you try and goto any website then your going to see a error message because the certificate that the Burp Suite proxy is using to resign the web packets after being intercepted is unknown to the android device.  We need to get and trust the proxies certificate.

This is down by navigating to the address of the proxy on the android device and you should see this page.

<div class='text-center'>
  <img src="{{ site.baseurl }}/assets/img/burp/CA_Site.png" alt='Site to download Burp certificate' />
</div>

Click the `CA Certificate` button and download the `cert.der` file.  This is the proxies certificate and once downloaded go and change the files name to `cert.cer` then click on it.  

<div class='text-center'>
  <img src="{{ site.baseurl }}/assets/img/burp/CA_Button.png" alt='Button to get Burp certificate' />
</div>

The phone should recognize it as a certficate and as if you want to trust it or in my phones case just name it and assign it to the wifi and vpn setting.

<div class='text-center'>
  <img src="{{ site.baseurl }}/assets/img/burp/Burp_Cert.png" alt='Burp Certificate Naming in Android' />
</div>

Now if we try to navigate to another webpage we should see the traffic being intercepted in the `Intercept` sub tab of `Proxy` on Burp.

<div class='text-center'>
  <img src="{{ site.baseurl }}/assets/img/burp/Burp_Capture.png" alt='Traffic captured by Burp' />
</div>

This is holding up the loading of the phone until you forward the request, if you drop it the page will never load because the request never made it to its destination.

You can view all the requests that have been intercepted by clicking on the proxies `HTTP History` sub tab.  Here you can view the details of each request and resend them if you want or alter them in any way.

<div class='text-center'>
  <img src="{{ site.baseurl }}/assets/img/burp/Burp_History.png" alt='Burps HTTP History tab screenshot' />
</div>

Once we forward the intercepted request and allow the response back the page will load.

<div class='text-center'>
  <img src="{{ site.baseurl }}/assets/img/burp/Rendered_Site.png" alt='Website loaded after being intercepted' />
</div>

Congratulations your now intercepting the traffic from that android device!  I hope this helped some people out in some way.  Don't forget to clear the proxy settings after finishing and maybe even delete that certificate from the device as well.
