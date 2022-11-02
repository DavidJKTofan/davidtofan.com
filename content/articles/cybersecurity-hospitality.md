---
title: Cybersecurity in Hospitality
date: 2022-11-01
images: 
- /media/articles/cybersecurity-hospitality.png
tags:
  - Security
  - CyberSec
  - Privacy
  - Cloudflare
categories:
  - Security
  - Privacy
---

## Cybersecurity in Hospitality

Even hotels/hostels/aparthotels or any other business in the hospitality industry requires cybersecurity. Actually, especially those, since they are dealing with lots of personal information on a daily basis.

### Application Security

To briefly point out the obvious again: your websites and servers require cybersecurity.

You can get started with advanced security and performance by setting up the [Cloudflare Business Plan](https://www.cloudflare.com/plans/business/) for only $200/month. 
Go check it out today! However, take into account that it's a self-serve plan, meaning that you will have to set up everything yourself.

If you require support, and enterprise-grade and customizable solutions, as well as [Data Localization Suite](https://www.cloudflare.com/data-localization/) – to control on a technical-level that visitor traffic's metadata which can identify a customer stays in the EU, and TLS termination and inspection happens only in EU data centers –, then get in touch with the Cloudflare team to talk about the [Enterprise Plan](https://www.cloudflare.com/enterprise/).

As an overview, the Enterprise Plan includes (among several other things):
- 24/7/365 email & emergency phone support
- Enterprise guided onboarding, training & continued 24/7/365 support
- Dependable service level agreements (SLA) with 100% uptime
- Advanced DDoS Protection
- Access common compliance documentation for topics like PCI, SOC 2, ISO, and more

### Compliance

In terms of compliance, most often many hospitality businesses are faced with the challenges of using (or not using) Google products, especially Google Analytics or Google Fonts.

See the overview of Cloudflare's [certifications and compliance resources](https://www.cloudflare.com/trust-hub/compliance-resources/).

#### Google Analytics

With [Cloudflare Zaraz](https://developers.cloudflare.com/zaraz/) you can quickly and easily set the following settings for Google Analytics and other third-party tools:
- Hide Originating IP Address (more info on the [DevDocs](https://developers.cloudflare.com/zaraz/faq/#after-moving-from-google-analytics-4-to-zaraz-i-can-no-longer-see-demographics-data-why))
- Remove URL query parameters
- Trim IP addresses
- Clean User Agent strings
- Remove external referrers

Therefore, answering and following the guidelines set out by the [French National Data Protection Authority (CNIL)](https://blog.cloudflare.com/zaraz-privacy-features-in-response-to-cnil/), the [Austrian Data Protection Authority (DSB)](https://blog.cloudflare.com/keep-analytics-tracking-data-in-the-eu-cloudflare-zaraz/), and others.

Read more on my other article [Cloudflare Zaraz](https://davidtofan.com/articles/cloudflare-zaraz/).

#### Google Fonts

According to the [TechnikNews](https://www.techniknews.net/en/news/datenschutzverletzung-wegen-google-fonts-datenschutzanwalt-mahnt-ab/) and several others, Google Fonts sends a website users' IP addresses to the US, which is unlawful towards GDPR.

The potential solutions are:

1. Locally host fonts

Download the fonts files and host them on your server, loading them straight from there, instead of another third-party server.

2. Disable Google Fonts altogether

Changing your Content-Security-Policy (CSP) to `content-security-policy: font-src 'self';`.

3. Get user consent before

Technically, you are allowed to use Google Fonts, if the website user has given explicit consent prior to them downloading and loading. The users should be informed that their IP addresses will be sent to Google servers to provide your website with Google Fonts.

Choose one option and stay up to date with the latest requirements and news.

#### Google reCAPTCHA

reCAPTCHA collects personal information and communicates with Google servers.

Meet [Cloudflare Turnstile](https://developers.cloudflare.com/turnstile/): a user-friendly, privacy-preserving alternative to CAPTCHA.

If you are already using reCAPTCHA today, [follow this tutorial to migrate](https://developers.cloudflare.com/turnstile/get-started/migrating-from-recaptcha/) seamlessly to Cloudflare Turnstile.

### Conclusion

Get cybersecurity and performance, and achieve compliance within only a few hours, with the support of Cloudflare.

Interested in some [demos](https://www.cf-testing.com/)?

Need help getting started with Cloudflare? Get a [personalized recommendation](https://www.cloudflare.com/about-your-website/).

* * * *

## Disclaimer

This is a very general introduction to Cybersecurity in Hospitality. Educational purposes only. Educational purposes only, and this blog post does not necessarily reflect the opinions of Cloudflare. There are many more aspects to Cloudflare and its products and services – this is merely a brief educational intro. Properly inform yourself, keep learning, keep testing, and feel free to share your learnings and experiences as I do. Hope it was helpful! Images are online and publicly accessible.