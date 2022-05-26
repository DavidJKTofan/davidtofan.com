---
title: Comparing Cloudflare
date: 2022-06-30
images: 
- /media/articles/website-security.png
tags:
  - Security
  - CyberSec
categories:
  - Security
draf: true
---

## Comparison Table


|  | **Cloudflare** | **Akamai** | **Zscaler** | **Cisco** | **Netskope** |
|:---:|:---:|---|:---:|:---:|---|
| **GENERAL** |  |  |  |  |  |
| Data centers | +270 | (+11 with Linode) | +150 | +37 |  |
| Network Interconnections | +10,500 | +1,450 |  |  |  |
| Network capacity | +142 Tbps | +300 Tbps | +1,5 Tbps | +1 Tbps | +100 Tbps |
| Ciphers | BoringSSL – TLS 1.3 and below |  | TLS 1.3 and below |  |  |
| Clients | ✅ – uses WireGuard |  | Different features depending on OS – uses Z-Tunnel |  |  |
| Traffic Forwarding Methods | Client, GRE Tunnel, IPSec Tunnel, PAC Files, Cloudflare Tunnel |  | Client, GRE Tunnel, IPSec Tunnel, PAC Files, Proxy Chaining, Zscaler Cloud Connector |  |  |
| **ZERO TRUST** |  |  |  |  |  |
| **Secure Web Gateway** | **Cloudflare Gateway** |  | **Zscaler Internet Access®** | **Cisco Umbrella** |  |
| DNS requests | ~200 Billion – Responsible for the F-root DNS nameserver |  | - | ~400 Billion |  |
| Unlimited SSL/TLS (HTTP) inspection | ✅ |  | ✅ |  |  |
| Web and URL filtering (incl. categories) | ✅ |  | ✅ |  |  |
| DNS filtering | ✅ |  | ✅ |  |  |
| Antivirus / Malware protection | ✅ |  | ✅ |  |  |
| Data Loss Prevention (DLP) | ✅ |  | ✅ |  |  |
| Automatic threat updates | ✅ |  | ✅ |  |  |
|  |  |  |  |  |  |
| **Access** |  |  |  |  |  |
|  |  |  |  |  |  |
|  |  |  |  |  |  |
|  |  |  |  |  |  |
| **Browser Isolation** | **Remote Browser Isolation** |  | **Cloud Browser Isolation** | **Cisco Umbrella remote browser isolation (RBI)** |  |
| Features | Disable keyboard, upload/download, copy-pasting, printing, and add watermark |  | Disable upload/download, copy-pasting, and printing |  |  |
| Clientless | ✅ |  | ✅ |  |  |
| Limitations | Webcam / Microphone support is unavailable Websites that use WebGL may not function |  | Maximum 10 browser tabs open Isolated browser URL address bar is read-only |  |  |
| **Email Security** | **Cloudflare Area 1** |  | **Zscaler Delivery Assurance (?)** | **IronPort** |  |
|  |  |  |  |  |  |


_NOTE: data as of April 2022._

**WORK IN PROGRESS...**


## Information Sources

**GENERAL**
- Cloudflare: https://www.cloudflare.com/network/ and https://developers.cloudflare.com/ssl/ssl-tls/cipher-suites/ 
- Zscaler: https://trust.zscaler.com/zscaler.net/data-center-map and https://www.zscaler.com/press/zscaler-powers-its-global-data-centers-and-offices-100-renewable-energy and https://help.zscaler.com/zia/supported-cipher-suites-ssl-inspection and https://help.zscaler.com/zpa/about-zpa-cloud-architecture and https://www.zscaler.com/blogs/company-news/2019-review
- Cisco: https://umbrella.cisco.com/why-umbrella/global-network-and-traffic and https://www.nojitter.com/video-collaboration-av/webex-meetings-or-zoom-peering-performance
- Akamai: https://www.akamai.com/company/facts-figures and https://www.datacenterknowledge.com/archives/2012/03/08/akamai-now-running-105000-servers and https://www.datacenterdynamics.com/en/news/cdn-company-akamai-enters-cloud-business-with-900m-linode-acquisition/ and https://www.akamai.com/our-thinking/cdn/what-is-a-cdn
- Netskope: https://www.bizety.com/2019/07/29/netskope-builds-50-pop-100-tbps-cdn-caliber-network/ 

**ZERO TRUST**
- Cloudflare: https://developers.cloudflare.com/warp-client/get-started/ and https://developers.cloudflare.com/workers/examples/data-loss-prevention/ and https://developers.cloudflare.com/cloudflare-one/policies/browser-isolation/ 
- Zscaler: https://www.zscaler.com/platform/zscaler-client-connector and https://help.zscaler.com/zia/about-data-loss-prevention and https://help.zscaler.com/zia/about-cloud-browser-isolation and https://www.zscaler.com/technology/browser-isolation and https://help.zscaler.com/zia/limits-cloud-browser-isolation 
- Cisco: https://umbrella.cisco.com/products/remote-browser-isolation 


### Other sources

- [Comparing Cloudflare vs. Zscaler](https://www.cloudflare.com/products/zero-trust/cloudflare-vs-zscaler/)
- [Worker Environments](https://workers.js.org/#state-of-worker-environments)
- [Choosing Between DNSFilter and Cloudflare Gateway?](https://www.dnsfilter.com/vs-cloudflare-gateway)
- [Compare the SASE Market](https://www.netify.com/lists/10-best-sase-vendors-comparison)

### Credits

Thank you [Tables Generator](https://www.tablesgenerator.com/markdown_tables#), friends, and colleagues.

* * * *

## Disclaimer

Educational purposes only.

This is a general comparison between Cloudflare and potential competitors or alternatives, using publicly available information. Educational purposes only, and this blog post does not necessarily reflect the opinions of Cloudflare, or any other entity mentioned here. 

There are many more aspects to Cloudflare and its products and services – this is merely a brief educational intro. Properly inform yourself, keep learning, keep testing, and feel free to share your learnings and experiences as I do. Hope it was helpful! Images are online and publicly accessible.