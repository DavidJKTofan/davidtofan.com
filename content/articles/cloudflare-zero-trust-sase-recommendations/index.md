---
title: General Zero Trust & SASE Recommendations
date: 2024-12-14
description: "This guide provides non-exhaustive recommendations and general best practices to achieve a comprehensive Zero Trust & SASE approach with Cloudflare."
tags: ["cybersecurity", "cloudflare", "resources", "zero trust", "sase"]
type: "article"
draft: true
---

## Introduction

This guide provides non-exhaustive recommendations and general best practices to achieve a comprehensive **Zero Trust or SASE approach** with [Cloudflare One](https://www.cloudflare.com/zero-trust/).

Some features mentioned are available only through add-on solutions, such as **Data Loss Prevention (DLP)** and **Remote Browser Isolation (RBI)**, or other **Enterprise** features, such as Gateway **Dedicated Egress IPs**.

For demonstration, this guide assumes a first-time, clean onboarding scenario to Zero Trust architecture with Cloudflare.

---

## Preparation

### Understanding SASE & Zero Trust

The primary difference between Zero Trust and Secure Access Service Edge (SASE) lies in scope:

- **Zero Trust**: Focuses on managing access and authorization for authenticated users based on the principle _"never trust, always verify"_.
- **SASE**: Encompasses broader network and security functionalities, integrating Zero Trust alongside other capabilities.

In Zero Trust, all networks are treated as insecure – whether corporate, home, or public Wi-Fi. This mindset simplifies security, reducing complexity and hidden costs associated with traditional models. This requires a change of perspective.

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

### Tailoring to Use Cases

Organizations embark on SASE and Zero Trust journeys differently. A detailed analysis of specific use cases and risks helps define the optimal approach and roadmap.

Here are some roadmap examples:

- [A vendor-agnostic Roadmap to Zero Trust Architecture](https://zerotrustroadmap.org/)
- [A Roadmap to Zero Trust Architecture – Whitepaper](https://cf-assets.www.cloudflare.com/slt3lc6tev37/9jyDLdW3VXPGwChDCCnrx/a4b7f7825e229f2caeaa1a12a2d16b24/Whitepaper_A-Roadmap-to-Zero-Trust-Architecture.pdf)
- [Roadmap to Zero Trust – theNET](https://www.cloudflare.com/the-net/roadmap-zerotrust/)
- [Security Transformation – Episode 1: The Roadmap to Zero Trust – theNET](https://www.cloudflare.com/the-net/security-transformation/zero-trust-roadmap/)
- [Cloudflare Zero Trust: A roadmap for highrisk organizations](https://cf-assets.www.cloudflare.com/slt3lc6tev37/4R2Wyj1ERPecMhbycOiPj8/c30f3e8502a04c6626e98072c48d4d7b/Zero_Trust_Roadmap_for_High-Risk_Organizations.pdf)
- [How to implement Zero Trust security – Learning](https://www.cloudflare.com/en-gb/learning/access-management/how-to-implement-zero-trust/)

### Understanding Connectivity

It’s critical to distinguish between connecting **locations/networks**, **users (knowledge workers)**, and **applications/resources**. 

The following table outlines Cloudflare's composable and flexible connectivity options:

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

> _Updated as of December 2024. Public Internet remains a viable connection option to the nearest Cloudflare data center._

### Modus Operandi

At Cloudflare there are two main policy rule builders:

- **Cloudflare Gateway**: Secure Web Gateway to inspect outbound traffic, which includes DNS, Network, and HTTP filtering, as well as Egress and Resolver Policies.
- **Cloudflare Access**: Zero Trust Network Access (ZTNA) authentication layer solution to secure inbound traffic to your applications.

Standardized naming conventions simplify policy management. This is especially relevant for Gateway. Additionally, be aware of the [order of enforcement](https://developers.cloudflare.com/cloudflare-one/policies/gateway/order-of-enforcement/).

Policies should be:

- Unique across your Cloudflare account.
- Structured for clarity and purpose.
- Descriptive (limited to 100 characters).

Policy Naming Convention model: `<Source>-<Cloudflare Component/Protocol>-<Destination>-<Purpose>`

Examples:

- `All-DNS-Domain-Whitelist`
- `All-DNS-Domain-BlacklistMalware`
- `<IdPGroup>-DNS-Domain-BlockAItools`
- `<IdPGroup>-NET-<DestinationIPList>-SAPAccess`
- `All-HTTP-SecurityRisks-CategoriesBlacklist`
- `All-HTTP-Application-DoNotInspectBankApps`
- `<IdPGroup>-HTTP-FileUpload-DoNotScan`

### Firewalls

Review and align current (device, on-prem or origin server) firewall configurations for Cloudflare integrations:

- [WARP with firewall](https://developers.cloudflare.com/cloudflare-one/connections/connect-devices/warp/deployment/firewall/)
- [Cloudflare Tunnel with firewall](https://developers.cloudflare.com/cloudflare-one/connections/connect-networks/deploy-tunnels/tunnel-with-firewall/)
- [Browser Isolation with firewall](https://developers.cloudflare.com/cloudflare-one/policies/browser-isolation/network-dependencies/)

### Limitations

Be aware and familiarize yourself with the general [account limits](https://developers.cloudflare.com/cloudflare-one/account-limits/), most of which are soft-limits, as well as any other feature-specific limitations or caveats, such as the [inspection limitations](https://developers.cloudflare.com/cloudflare-one/policies/gateway/http-policies/tls-decryption/#inspection-limitations).

----

## Getting Started

### Standard Configurations

[Create a Zero Trust organization](https://developers.cloudflare.com/cloudflare-one/setup/#create-a-zero-trust-organization) by choosing a team name (`<your-team-name>`) for your organization/account.

[Enable the Gateway proxy](https://developers.cloudflare.com/cloudflare-one/policies/gateway/proxy/#proxy-protocols), specifically UDP & ICMP. This ensures support for routing all application traffic types, including [HTTP/3 inspection](https://developers.cloudflare.com/cloudflare-one/policies/gateway/http-policies/http3/).

[Enable TLS Decryption](https://developers.cloudflare.com/cloudflare-one/policies/gateway/http-policies/tls-decryption/). This is required to apply HTTP filtering, such as Content Categorization and Scanning.

[Enable AV Scanning](https://developers.cloudflare.com/cloudflare-one/policies/gateway/http-policies/antivirus-scanning/#enable-av-scanning). This adds an additional layer of security by allowing to scan the uploaded and downloaded files for malware. For larger files that cannot be scanned, consider allowing [Do Not Scan](https://developers.cloudflare.com/cloudflare-one/policies/gateway/http-policies/#do-not-scan) exceptions if necessary.

[Set up Clientless Browser Isolation](https://developers.cloudflare.com/cloudflare-one/policies/browser-isolation/setup/clientless-browser-isolation/#set-up-clientless-web-isolation). This can be useful for high-risk users, or external providers or third-party users who require controlled access to certain internal applications.

[Configure your Identity Providers (IdP)](https://developers.cloudflare.com/cloudflare-one/identity/idp-integration/). Here it is also recommended to use [SCIM provisioning](https://developers.cloudflare.com/cloudflare-one/identity/users/scim/) to sync users and groups, simplifying user management and deprovisioning.

[Configure the Device Enrollment Permissions](https://developers.cloudflare.com/cloudflare-one/connections/connect-devices/warp/deployment/device-enrollment/) to only allow approved user devices to join your organization. Related to this, it is recommended to [install the root certificate using WARP](https://developers.cloudflare.com/cloudflare-one/connections/connect-devices/user-side-certificates/) and trust it, if possible.

[Configure the App Launcher](https://developers.cloudflare.com/cloudflare-one/applications/app-launcher/) to provide users with a centralized dashboard of authorized applications.

[Improve end user experience](https://developers.cloudflare.com/cloudflare-one/policies/gateway/block-page/#customize-the-block-page) by customizing the Block Page and applying your own corporate branding. Check out this open source _highly customizable block page built in Cloudflare Workers that provides enriched Access Deny reasoning to end users_ on [GitHub](https://github.com/cloudflare/cf-identity-dynamic). Be sure to enable Block Page support for DNS and HTTP policies.

[Create or upload your Lists](https://developers.cloudflare.com/cloudflare-one/policies/gateway/lists/) of URLs, hostnames, or other entries to later use in the Gateway or Access Policies.

Create [Access Groups](https://developers.cloudflare.com/cloudflare-one/identity/users/groups/) for quick and reusable groups within policies.

Create different [Lists](https://developers.cloudflare.com/cloudflare-one/policies/gateway/lists/) so you can quickly use them in future policies. 

Configure [SSH command logging](https://developers.cloudflare.com/cloudflare-one/connections/connect-networks/use-cases/ssh/ssh-infrastructure-access/#ssh-command-logs) with your own public key to encrypt logged SSH commands that users ran on a target machine.

----

## Connecting Locations / Networks

### DNS Resolver IPs

TBD 

### DNS over HTTPS (DoH)

TBD 

### Cloudflare Tunnel

TBD 

### WARP Connector

TBD 

### MagicWAN

TBD 

### Cloudflare Network Interconnect (CNI)

TBD 

----

## Connecting Applications

### Cloudflare Tunnel

TBD

### WARP Connector

TBD

----

## Connecting Users

### WARP Client (Recommended)

TBD

### DNS over HTTPS (DoH)

TBD 

### Proxy Endpoint (PAC file)

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

Policy Naming Convention model: `<Source>-<Cloudflare Component/Protocol>-<Destination>-<Purpose>`

### DNS Policies

#### TBD

TBD

<IMAGE_HERE>

Reference: TBD.

#### More Examples for DNS Policies

[Recommended DNS policies](https://developers.cloudflare.com/learning-paths/secure-internet-traffic/build-dns-policies/recommended-dns-policies/).

### Network Policies

#### TBD

TBD 

<IMAGE_HERE>

Reference: TBD.

#### More Examples for Network Policies

[Recommended Network policies](https://developers.cloudflare.com/learning-paths/secure-internet-traffic/build-network-policies/recommended-network-policies/).

### HTTP Policies

#### TBD

TBD

<IMAGE_HERE>

Reference: TBD.

#### More Examples for HTTP Policies

[Recommended HTTP policies](https://developers.cloudflare.com/learning-paths/secure-internet-traffic/build-http-policies/recommended-http-policies/).

### Resolver Policies

TBD

<IMAGE_HERE>

Reference: TBD.

### Egress Policies

TBD

<IMAGE_HERE>

Reference: TBD.

----

## Insights & Monitoring

### Shadow IT

TBD 

### Digital Experience Monitoring (DEX)

TBD

---

## Automation

### API & Terraform

Infrastructure automation tools enable developers to seamlessly integrate Zero Trust security into their application development pipelines.  

For larger organizations, leveraging [Terraform](https://developers.cloudflare.com/cloudflare-one/api-terraform/) for automated deployment is highly recommended. Additionally, the **Dashboard Read-Only** functionality provides secure, audit-friendly access for monitoring and oversight.

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
