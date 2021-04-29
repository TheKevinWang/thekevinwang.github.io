---
title:  "Google Open Redirect"
categories: 
  - web
tags:
  - open redirect
  - phishing
  - google
toc: false
---

Edit: Google fixed this by introducing a redirect warning page that requires the user to click the redirected link.

https://www.google.com/url?sa=D&q=https://clemson.edu

This can be combined with the normal google redirect, which usually displays the link and the user has to manually click the url, to skip that and automatically redirect, as google.com is a whitelisted domain. 

https://www.google.com/url?q=https://appengine.google.com/_ah/logout?continue=https://clemson.edu

A redirection notice is flashed for less than half a second. Most users will not suspect anything. 

This is especially useful for mobile phishing, as the user only sees the "google.com" and "Redirecting," along with Google's logo in the preloaded message. 

I have contacted Google regarding this "vulnerability," but Google declined to classify it as such.  Google's policy on open redirects is to monitor and control rather than completely prevent.
