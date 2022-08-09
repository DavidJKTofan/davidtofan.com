---
title: Cloudflare Zaraz
date: 2022-08-14
images: 
- /media/articles/cloudflare-zaraz.png
tags:
  - Zaraz
  - Web Development
  - CyberSec
  - Analytics
categories:
  - Privacy
  - Developers
---

## Cloudflare Zaraz

Zaraz loads third-party tools in the cloud, away from browsers, improving speed, security, and privacy, giving you full control over third-party tools without (necessarily) changing any code on your website.

Here some interesting reads:
* [Cloudflare Zaraz Product Page](https://www.cloudflare.com/products/zaraz/)
* [Zaraz Developer Docs](https://developers.cloudflare.com/zaraz/)
* [Need to keep analytics data in the EU? Cloudflare Zaraz can offer a solution](https://blog.cloudflare.com/keep-analytics-tracking-data-in-the-eu-cloudflare-zaraz/)
* [Cloudflare Zaraz supports CSP](https://blog.cloudflare.com/cloudflare-zaraz-supports-csp/)

## Practical Examples

Several examples and best practices are described in the Zaraz Developer Docs in the [triggers and rules](https://developers.cloudflare.com/zaraz/reference/triggers/) section.

### Google Analytics 4

By going on the [Privacy](https://dash.cloudflare.com/?to=/:account/:zone/zaraz/settings) settings and toggling-on all those options, as well as toggling-on "Hide Originating IP Address" in the Tools' Settings of Google Analytics 4 itself ensures that you are taking a proactive approach towards privacy and some regulations and standards.

Here an interesting read: [Cloudflare Zaraz launches new privacy features in response to French CNIL standards](https://blog.cloudflare.com/zaraz-privacy-features-in-response-to-cnil/).

### Google Fonts

_The [Web Font Loader](https://github.com/typekit/webfontloader) is a JavaScript library that gives you more control over font loading than the Google Fonts API provides._

An example for a **Custom HTML** Tool Action with PageView Trigger to load **Google Fonts** is the following:
```
<script>
  console.log("Google Fonts injected via Zaraz!");

  WebFontConfig = {
      google: {
          families: ["Ubuntu", 
            "Lato:100,300,400,700,900", 
            "Montserrat:100,300&display=swap"],
      },
      loading: function() {
          console.log("Fonts are being loaded");
      },
      active: function() {
          console.log("Fonts have been rendered")
      }
  };

  (function() {
      var wf = document.createElement("script");
      wf.src = ("https:" == document.location.protocol ? "https" : "http") + "://ajax.googleapis.com/ajax/libs/webfont/1/webfont.js";
      wf.type = "text/javascript";
      wf.async = "true";
      var t = document.getElementsByTagName("script")[0];
      t.parentNode.insertBefore(wf, t);
  })(); 
</script>
```

### Custom System Properties and Triggers

Setting the rule type `Match rule` with variable `{{ system.page.url.pathname }}`, match operation `Starts with`, and match string i.e. `/index`, allows a tool to be triggered on specific URL pathnames.

Check out all [Zaraz event and system properties](https://developers.cloudflare.com/zaraz/reference/properties-reference/).

### Block Zaraz

In order to prevent Zaraz to be loaded on specific pages, one can use [Page Rules](https://developers.cloudflare.com/zaraz/advanced/block-zaraz/).

Additionally, one can define [Blocking Triggers](https://developers.cloudflare.com/zaraz/advanced/blocking-triggers/) to define when to _not_ start an action.

Alternatively, one can decide to turn off the [Auto-inject script](https://developers.cloudflare.com/zaraz/reference/settings/#auto-inject-script) and only [load Zaraz manually](https://developers.cloudflare.com/zaraz/advanced/load-zaraz-manually/) on specific sites by adding the following `<script>` tag to your header:
```
<html>
  <head>
    ...
    <script src="/cdn-cgi/zaraz/i.js" referrerpolicy="origin"></script>
  </head>
  <body>
    ...
  </body>
</html>
```

* * * *

## Disclaimer

This is a very general introduction to Website security, specifically to Content Security Policy (CSP). Educational purposes only. There are many more aspects to website security which are not covered here, and all implementations might differ from situation to situation, technology to technology. Remember, you are responsible for your own actions – this is merely an educational intro. Properly inform yourself, keep learning, keep testing, and feel free to share your learnings and experiences as I do. Hope it was helpful!
