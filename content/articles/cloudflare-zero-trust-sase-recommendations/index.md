---
title: General Application Security Recommendations
date: 2024-12-12
description: "This guide provides non-exhaustive recommendations and general best practices to achieve a comprehensive L7 Application Security approach with Cloudflare."
tags: ["cybersecurity", "cloudflare", "resources", "zero trust", "sase"]
type: "article"
draft: true
---

## Introduction

This guide provides non-exhaustive recommendations and general best practices to achieve a comprehensive **Zero Trust or SASE approach** with [Cloudflare One](https://www.cloudflare.com/zero-trust/).

Some features mentioned are available only through add-on solutions, such as **Data Loss Prevention (DLP)** and **Remote Browser Isolation (RBI)**, or other **Enterprise** features, such as Gateway **Dedicated Egress IPs**.

For the sake of demonstration, we'll be going over the majority of scenarios when taking the first step into the journey towards Zero Trust architecture implementation with Cloudflare, pretending like this is a clean first time onboarding.

---

## Preparation

### Understanding SASE & Zero Trust

The primary distinction between Zero Trust and Secure Access Service Edge (SASE) lies in their scope. Zero Trust focuses on a strategy for managing access and authorization controls for authenticated users, whereas SASE encompasses a wider and more intricate range of functionalities. SASE integrates extensive network and security services, including Zero Trust as one of its components.

> The main concept behind the Zero Trust security model is _"never trust, always verify"_, which means that networks, workloads, users, and devices should not be trusted by default.

All this requires a change of perspective: every network should be considered insecure, even your own corporate one. It does not matter if the user is sitting in the office, at home, at the beach, or at a local coffee; every network should not be trusted. Interestingly, this can reduce the complexity in security posture, as well as identifying hidden costs that the prior complexity was doing.

There are plenty of articles about these topics going into much more detail. Feel free to do your own research or check out these blog posts:

- [Zero Trust, SASE and SSE: foundational concepts for your next-generation network](https://blog.cloudflare.com/zero-trust-sase-and-sse-foundational-concepts-for-your-next-generation-network/).
- [Zero Trust](https://www.bsi.bund.de/EN/Themen/Unternehmen-und-Organisationen/Informationen-und-Empfehlungen/Zero-Trust/zero-trust_node.html)
- [SASE vs. ZTNA: What Is the Difference?](https://www.paloaltonetworks.com/cyberpedia/sase-vs-ztna).
- [SASE vs. Zero Trust: What's the Difference?](https://www.zscaler.com/de/blogs/product-insights/sase-vs-zero-trust-what-s-difference).

Some general advantages of SASE usually are:

- **Comprehensive Security**: Combines network and security services in a unified platform.  
- **Zero Trust Integration**: Incorporates Zero Trust principles for secure access.  
- **Scalability**: Adapts to growing and evolving organizational needs.  
- **Cloud Optimization**: Improves performance for cloud-based applications.  
- **Simplified Management**: Centralizes control for reduced complexity.  
- **Enhanced Performance**: Optimizes traffic routing for faster connectivity.  
- **Global Reach**: Ensures consistent security policies across distributed locations.  

### It's all about the Use Cases

Every organization starts the journey towards SASE and Zero Trust differently and the roadmap varies depending on different needs and risks.

Diving into the required use cases in detail determines what the best approach and roadmap are.

### Understanding Connectivity

It is important to differentiate between locations/networks VS users VS applications/resources.

Here is an overview of composable and flexible on-ramps and off-ramps for connecting to or from the Cloudflare network:

| **Connectivity**                      	|        **Locations (Networks)**       	|                              **Users**                              	| **Applications** 	|   	|
|---------------------------------------	|:-------------------------------------:	|:-------------------------------------------------------------------:	|:----------------:	|---	|
| WARP Client (default mode)            	|                                       	|                                  ✅                                  	|                  	|   	|
| WARP Client (Proxy mode)              	|                                       	|                                  ✅                                  	|                  	|   	|
| DNS Resolver IPs                      	|                   ✅                   	|                                                                     	|                  	|   	|
| DNS over HTTPS (DoH)                  	| ✅<br>(location-specific DoH endpoint) 	| ✅<br>(requires user-specific DoH token for identity-based policies) 	|                  	|   	|
| Proxy Endpoint (PAC file)             	|                                       	|           ✅<br>(does not support identity-based policies)           	|                  	|   	|
| Cloudflare Tunnel                     	|                   ✅                   	|                                                                     	|         ✅        	|   	|
| WARP Connector                        	|                   ✅                   	|                                                                     	|         ✅        	|   	|
| Clientless RBI                        	|                                       	|                                  ✅                                  	|                  	|   	|
| Magic WAN                             	|                   ✅                   	|                                                                     	|         ✅        	|   	|
| Cloudflare Network Interconnect (CNI) 	|                   ✅                   	|                                                                     	|         ✅        	|   	|

> _This information is as of December 2024. Note, one can of course still use the public Internet as a form of connectivity to the closest Cloudflare data center._

### Modus Operandi

It is generally suggested to use a standard naming convention when building policies for users.

- Policy Names should be unique across the entire Cloudflare Account.
- Policy Names should follow the same structure, to make it easier understand the purpose of it.
- Policy Names should be descriptive, but also limited in length (max. 100 char).

Policy Naming Convention model: `<Source-Name>-<CF-Component/Protocol>-<Destination-Selector>-<Purpose>`

Examples:

- `All-DNS-Domain-Whitelist`
- `<IdPGroup>-NET-<DestinationIPList>-SAPAccess`
- `All-HTTP-SecurityRisks-CategoriesBlacklist`

### Firewalls

Review your current (on-prem or origin server) firewall configurations:

- [WARP with firewall](https://developers.cloudflare.com/cloudflare-one/connections/connect-devices/warp/deployment/firewall/)
- [Cloudflare Tunnel with firewall](https://developers.cloudflare.com/cloudflare-one/connections/connect-networks/deploy-tunnels/tunnel-with-firewall/)
- [Browser Isolation with firewall](https://developers.cloudflare.com/cloudflare-one/policies/browser-isolation/network-dependencies/)

----

## Getting Started

### Standard Configurations

[Create a Zero Trust organization](https://developers.cloudflare.com/cloudflare-one/setup/#create-a-zero-trust-organization) by choosing a team name (`<your-team-name>`) for your organization/account.

[Enable the Gateway proxy](https://developers.cloudflare.com/cloudflare-one/policies/gateway/proxy/#proxy-protocols), specifically UDP & ICMP. This is required to route any traffic type of application.

[Enable TLS Decryption](https://developers.cloudflare.com/cloudflare-one/policies/gateway/http-policies/tls-decryption/). This is required to apply HTTP filtering, such as Content Categorization and Scanning.

[Enable AV Scanning](https://developers.cloudflare.com/cloudflare-one/policies/gateway/http-policies/antivirus-scanning/#enable-av-scanning). This adds an additional layer of security by allowing to scan the uploaded and downloaded files for malware. In general, you might want to allow files that cannot be scanned if your workforce is working with larger file sizes.

[Set up Clientless Browser Isolation](https://developers.cloudflare.com/cloudflare-one/policies/browser-isolation/setup/clientless-browser-isolation/#set-up-clientless-web-isolation). This can be useful for high-risk users, or external providers or third-party users who require controlled access to certain internal applications.

[Configure your Identity Providers (IdP)](https://developers.cloudflare.com/cloudflare-one/identity/idp-integration/). Here it is also recommended to use [SCIM provisioning](https://developers.cloudflare.com/cloudflare-one/identity/users/scim/), in order to sync users and groups in Zero Trust policies. This is especially useful for deprovisionining of users.

[Configure the Device Enrollment Permissions](https://developers.cloudflare.com/cloudflare-one/connections/connect-devices/warp/deployment/device-enrollment/) to only allow specific user devices to join your organization.

(Optionally) [configure the App Launcher](https://developers.cloudflare.com/cloudflare-one/applications/app-launcher/), which displays applications users are authorized to use: a centralized dashboard for all applications.

(Optionally) [improve end user experience](https://developers.cloudflare.com/cloudflare-one/policies/gateway/block-page/#customize-the-block-page) by customizing the Block Page and applying your own corporate branding. Check out this open source _highly customizable block page built in Cloudflare Workers that provides enriched Access Deny reasoning to end users_ on [GitHub](https://github.com/cloudflare/cf-identity-dynamic). Don't forget to enable Block Page for your DNS and HTTP Block Policies.

----

## Connecting Applications

### Cloudflare Tunnel

TBD

### WARP Connector

TBD

----

## Connecting Users

### Deploying the WARP Client

TBD

### Clientless Remote Browser Isolation (RBI)

TBD

For Clientless RBI, they'll navigate any URL via: `https://<your-team-name>.cloudflareaccess.com/browser/<URL>`.

### DNS over HTTPS (DoH)

This is an agentless option. 

With Cloudflare Gateway, you can filter [DNS over HTTPS (DoH)](https://developers.cloudflare.com/cloudflare-one/connections/connect-devices/agentless/dns/dns-over-https/#filter-doh-requests-by-user) requests by user with user-specific DoH tokens, applying identity-based policies.

### Proxy Endpoint (PAC file)

This is an agentless option. 

You can apply Gateway HTTP and DNS policies at the browser level by configuring a [Proxy Auto-Configuration (PAC) file](https://developers.cloudflare.com/cloudflare-one/connections/connect-devices/agentless/pac-files/). 

----

## Creating Policies

### DNS Policies

TBD

<IMAGE_HERE>

Reference: TBD.

### Network Policies

TBD 

<IMAGE_HERE>

Reference: TBD.

### HTTP Policies

TBD

<IMAGE_HERE>

Reference: TBD.

----

## TBD


---

## Disclaimer

Educational purposes only.

This blog post is independently created and is not affiliated with, endorsed by, or necessarily representative of the views or opinions of any organizations or services mentioned herein.

The images used in this article primarily consist of screenshots from the Cloudflare Dashboard or other publicly available materials, such as Cloudflare webinar slides.

The guidelines provided in this post are intended for general educational purposes. They should be customized to fit your specific use cases and traffic patterns. You are responsible for configuring settings according to your unique requirements, and it is important to understand their potential impact. Familiarity with Cloudflare concepts and relevant features is recommended.

The author of this post is not responsible for any misconfigurations, errors, or unintended consequences that may arise from implementing the guidelines or recommendations discussed herein. You assume full responsibility for any actions taken based on this content and for ensuring that configurations are appropriate for your specific environment.

For additional learning resources, explore the following:

- [Learning Paths](https://developers.cloudflare.com/learning-paths/)
- [Implementation Guides](https://developers.cloudflare.com/reference-architecture/implementation-guides/zero-trust/)
- [Reference Architectures](https://developers.cloudflare.com/reference-architecture/by-solution/#zero-trust--sase)
- [Enterprise Customer Portal](https://www.cloudflare.com/ecp/overview/) (for Enterprise customers)
- [Security Center](https://developers.cloudflare.com/security-center/)

If you have any questions or need assistance, please [contact Cloudflare support](https://developers.cloudflare.com/support/contacting-cloudflare-support/#methods-of-contacting-cloudflare-support).
