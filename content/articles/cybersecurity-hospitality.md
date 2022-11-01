---
title: Cybersecurity in Hospitality
date: 2022-11-01
images: 
- /media/articles/cybersecurity-hospitality.png
tags:
  - Security
  - CyberSec
categories:
  - Security
  - Privacy
---

## Cybersecurity in Hospitality

Even hotels/hostels/aparthotels or any other business in the hospitality industry requires cybersecurity. Actually, especially those, since they are dealing with lots of personal information on a daily basis.

### Application Security

To briefly point out the obvious again: your websites and servers require cybersecurity.

With the [Cloudflare Business Plan](https://www.cloudflare.com/plans/business/) for only $200/month, you can enjoy advanced security and performance. Go check it out today and get started within the hour!

If you require enterprise-grade and customizable solutions, as well as [Data Localization Suite](https://www.cloudflare.com/data-localization/) – to also control on a technical-level that visitor traffic's metadata that can identify a customer stays in the EU, and TLS termination and inspection happens only in EU data centers –, then get in touch with the Cloudflare team to talk about the [Enterprise Plan](https://www.cloudflare.com/enterprise/).

### Compliance

In terms of compliance, most often many hospitality businesses are faced with the challenge of using (or not using) Google products, especially Google Analytics or Google Fonts.

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

* * * *

## Disclaimer

This is a very general introduction to Cybersecurity in Hospitality. Educational purposes only. Educational purposes only, and this blog post does not necessarily reflect the opinions of Cloudflare. There are many more aspects to Cloudflare and its products and services – this is merely a brief educational intro. Properly inform yourself, keep learning, keep testing, and feel free to share your learnings and experiences as I do. Hope it was helpful! Images are online and publicly accessible.