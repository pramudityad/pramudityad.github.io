---
title: "Setup Google Analytics Jekyll"
categories:
  - Tech
tags:
  - google analytics
  - minimal mistake
---

If you own website/blog/portfolio site or anything that can be access in url and you want to track the traffic google this article maybe can help you.

In this article will be show how to set up analytics for a website (Universal Analytics) based on this [Link](https://support.google.com/analytics/answer/10269537?hl=en) 

## Google Analytics
Google Analytics is a web analytics service offered by Google that tracks and reports website traffic, currently as a platform inside the Google Marketing Platform brand.

# Create an Analytics account
Your first step is to set up an Analytics account, unless you already have one. Skip to creating a property unless you want to create a separate account for this website. 

1. In Admin, in the Account column, click Create Account.
2. Provide an account name. Configure the data-sharing settings to control which data you share with Google.
3. Click Next to add the first property to the account.

# Create a property
1. Enter a name for the property.
2. Click Show advanced options
<img src="{{ site.url }}{{ site.baseurl }}/assets/images/google-analytics/g1.png" alt="">
3. Turn on the switch for Create a Universal Analytics property. 
<img src="{{ site.url }}{{ site.baseurl }}/assets/images/google-analytics/g2.png" alt="">

4. Enter the website URL. Select the protocol (http or https).
5. At this point choose to create
<img src="{{ site.url }}{{ site.baseurl }}/assets/images/google-analytics/g3.png" alt="">

6. Click Next
7. Click Create
<img src="{{ site.url }}{{ site.baseurl }}/assets/images/google-analytics/g4.png" alt="">


## Setup on you Jekyll
Im using minimal mistake theme, basically what we doing just modify `_config.yml`

Add this code to your config file
```
# Analytics
analytics:
  provider               : google-gtag # false (default), "google", "google-universal", "google-gtag", "custom"
  google:
    tracking_id          : 
    anonymize_ip         : false # true, false (default)
```
copy and paste your tracking ID in the file 

---
If everting already setup you can check you dashboard 
<img src="{{ site.url }}{{ site.baseurl }}/assets/images/google-analytics/g5.jpg" alt="">






