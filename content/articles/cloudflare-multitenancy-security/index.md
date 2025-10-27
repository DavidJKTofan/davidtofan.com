---
title: Multitenancy Security
date: 2025-10-26
description: "Comprehensive blueprint for securing SaaS and multi-tenant platforms on Cloudflare."
tags:
  [
    "cybersecurity",
    "cloudflare",
    "multitenancy",
    "multi tenancy",
    "tenants",
    "saas",
    "platform",
  ]
type: "article"
---

## Tenant-Aware Security for SaaS and Platform Providers

Modern SaaS and platform ecosystems rely on multi-tenant architectures – single infrastructures serving many distinct customers. This design maximizes scalability and efficiency but also centralizes risk: one vulnerability can impact all tenants. A tenant-based security strategy isolates risks, enforces per-customer policy, and maintains predictable control across layers of the stack.

This blueprint defines a practical Cloudflare-based approach to [multitenant](https://www.cloudflare.com/learning/cloud/what-is-multitenancy/) security framework.

## 1. Foundational Layer: Account-Wide / Global Controls

All platforms begin with shared baseline controls. These global defenses must protect every tenant (user or customer) equally.

**Recommended Controls:**

- **[Account-level WAF](https://developers.cloudflare.com/waf/account/):** Apply managed and custom rulesets at the account layer for consistent protection across all zones and domains.

![Account-level WAF example](img/account-level-waf.png)

- **Global Rate Limiting:** Mitigate volumetric or brute-force attacks at the edge before they reach origin.

![Catch-All Origin Server Infrastructure Capacity Rate Limiting Rule](img/catch-all-infrastructure-capacity-rate-limit.png)

- **Gateway DNS Policies:** Apply DNS and egress filtering for [Managed Service Providers (MSPs)](https://developers.cloudflare.com/cloudflare-one/traffic-policies/managed-service-providers/) and platform operators.

![Gateway DNS Filtering Policy](img/gateway-dns-policy-security-categories.png)

These global measures define the shared defense surface, ensuring platform-wide protection regardless of tenant segmentation.

## 2. Tenant Isolation at Application Layer

SaaS applications vary in exposure and tenant structure, some examples include: custom hostnames, APIs, user identity tokens, and subscription tiers. Granular enforcement becomes mandatory.

### No Predefined Tenant Identifiers

**Example scenario:** When tenants share a **common endpoint (e.g., `api.saas.com`)**, identifiers must be set based on specific logic or derived dynamically.

If tenants are not clearly identified by default, establish identifiers through the following methods:

- **Hostname Group [Lists](https://developers.cloudflare.com/waf/tools/lists/#availability):** Group hostnames (via request host header [`http.host`](https://developers.cloudflare.com/ruleset-engine/rules-language/fields/reference/http.host/)) by subscription tier (i.e. `free`). For example, a list named `free_tier_hostname_group` may include `customer1.saas.com` and `customer2.saas.com`.
- **[Transform Rules](https://developers.cloudflare.com/rules/transform/):** Append tenant-specific identifiers as query strings (`?tenant=enterprise`) or response headers (`X-Customer-Tier: enterprise`) using supported [filter expressions](https://developers.cloudflare.com/rules/transform/url-rewrite/reference/fields-functions/). Refer to the [phases list](https://developers.cloudflare.com/ruleset-engine/reference/phases-list/) to ensure proper rule execution order.
- **Rate Limiting by Header Value Of:** Apply rate limits based on a shared request header such as `api-key`, using the [characteristics](https://developers.cloudflare.com/waf/rate-limiting-rules/parameters/#with-the-same-characteristics) parameter.
- **Rate Limiting by Response Header:** [Increment counters](https://developers.cloudflare.com/waf/rate-limiting-rules/parameters/#increment-counter-when) based on response headers (e.g., `X-Tenant-ID: TenantID#1234`), added by origin server / backend logic.

### Vanity / Custom Hostnames as Tenant-Identifier

**Example scenario:**

- Hostname: `customer1.saas.com` / `saas.customer1.com` (vanity domain pointing to the SaaS provider)
- Custom Metadata: `"custom_metadata": { "customer_id": "12345", "redirect_to_https": true, "security_tag": "low", "tier": "free" }`
- Related Case Study: [Shopify](https://www.cloudflare.com/case-studies/shopify/)

Using **[Cloudflare for SaaS](https://developers.cloudflare.com/cloudflare-for-platforms/cloudflare-for-saas/)**:

- Use the request host header [**`http.host`**](https://developers.cloudflare.com/ruleset-engine/rules-language/fields/reference/http.host/) as a tenant signal in WAF and rate-limiting expressions.

![Cloudflare for SaaS WAF Hostname](img/saas-waf-host-header.png)

```bash
curl https://customer2.cf-saas.com
```

- Attach per-tenant [**Custom Metadata**](https://developers.cloudflare.com/cloudflare-for-platforms/cloudflare-for-saas/domain-support/custom-metadata/) to custom hostnames, then reference metadata in expressions for tier-based or customer-specific logic:

```
lookup_json_string(cf.hostname.metadata, "tier") eq "enterprise"
```

![Cloudflare for SaaS WAF Custom Metadata](img/saas-waf-custom-metadata.png)

```bash
curl https://customer1.cf-saas.com
```

> Custom Metadata is evaluated before the WAF [phase](https://developers.cloudflare.com/ruleset-engine/reference/phases-list/), making it the only viable option for attaching metadata that can be directly consumed by security rules.

### JWT-Based Controls

**Example scenario:**

- Hostname: `api.saas.com` (general shared endpoint)
- Request Header: `Authorization: Bearer wyJhbGc...` (includes JWT claims from the [JSON Web Key (JWK)](https://mkjwk.org/))

Authenticate and segment API users using JSON Web Token (JWT) claims:

- **API Shield [JWT Validation](https://developers.cloudflare.com/api-shield/security/jwt-validation/):** Validate tokens directly at the edge. For custom validation logic, use [Workers](https://developers.cloudflare.com/api-shield/security/jwt-validation/jwt-worker/) or lightweight [Snippets](https://developers.cloudflare.com/rules/snippets/examples/jwt-validation/).

![API JWT Validation Rule](img/api-jwt-validation-rule.png)

```bash
curl -I https://petstore.automatic-demo.com/api/v3/pet/findByStatus
```

- **Advanced Rate Limiting:** Enforce thresholds by [custom JWT claims](https://developers.cloudflare.com/waf/rate-limiting-rules/parameters/#requirements-for-using-claims-inside-a-json-web-token-jwt), such as `tenant_id` or `user_tier` claims.

![Advanced Rate Limiting JWT claim](img/advanced-rate-limit-jwt-claim.png)

- **[Transform Rules](https://developers.cloudflare.com/api-shield/security/jwt-validation/transform-rules/)**: Forward claims (e.g., `tenant_id`) as HTTP headers to origin server / backend systems for additional validation or custom logic.

### Request Header or API-Key Identification

**Example scenario:**

- Hostname: `api.saas.com` (general shared endpoint)
- Common Request Header: `api-key`

When tenants are distinguished via request headers:

- Inspect [**request headers**](https://developers.cloudflare.com/ruleset-engine/rules-language/fields/reference/?search-term=request&field-category=Headers) such as `X-Tenant-ID` or `X-User-ID` to apply selective WAF and rate-limiting policies.
- Configure rate limiting using the **[characteristics](https://developers.cloudflare.com/waf/rate-limiting-rules/parameters/#with-the-same-characteristics) `Header Value Of`** `api-key` field for consistent per-tenant per-API-Key throttling.

![Advanced Rate Limiting Header Value Of API-KEY](img/advanced-rate-limit-request-header-api-key.png)

```bash
for i in {1..50}; do
  curl -s -o /dev/null -w "%{http_code}\n" -H "api-key: something" -I "https://petstore.automatic-demo.com/api/v3/"
done
```

### Request Payload / Body

**Example scenario:**

- Hostname: `api.saas.com`
- Body: `{"identifier":"TenantID#123", "data1":"content_here"}`

When tenant identifiers are embedded in HTTP request body content ([**raw body**](https://developers.cloudflare.com/ruleset-engine/rules-language/fields/reference/?field-category=Body)):

- Use [**`lookup_json_string`** ](https://developers.cloudflare.com/ruleset-engine/rules-language/functions/#lookup_json_string) to extract tenant JSON fields for conditional matching:

```
lookup_json_string(http.request.body.raw, "identifier") eq "TenantID#123"
```

- Additional body sources include [**`http.request.body.multipart`**](https://developers.cloudflare.com/ruleset-engine/rules-language/fields/reference/http.request.body.multipart/) and [**`http.request.body.form`**](https://developers.cloudflare.com/ruleset-engine/rules-language/fields/reference/http.request.body.form/), useful for structured data submissions.

## 3. Tenant-Level Observability

Security enforcement is only as effective as its visibility. Observe tenant-specific behavior to maintain isolation and accountability.

- **[Security Analytics](https://developers.cloudflare.com/waf/analytics/security-analytics/) and [Security Events](https://developers.cloudflare.com/waf/analytics/security-events/):** Track attack vectors and request patterns, and depending on the identifier also per tenant.
- **[Log Explorer](https://developers.cloudflare.com/log-explorer/):** Query logs directly within the Cloudflare dashboard or create [custom dashboard](https://developers.cloudflare.com/log-explorer/custom-dashboards/) by tenant.
- **[Logpush](https://developers.cloudflare.com/logs/logpush/) with [Custom Fields](https://developers.cloudflare.com/logs/logpush/logpush-job/custom-fields/):** Export tenant identifiers or headers for correlation in external SIEM systems.
- **[GraphQL Analytics API](https://developers.cloudflare.com/analytics/graphql-api/):** Build dashboards filtered by hostname, metadata, or tenant ID for real-time insight.
- **[Custom Hostname Analytics](https://developers.cloudflare.com/cloudflare-for-platforms/cloudflare-for-saas/hostname-analytics/):** Available for Cloudflare for SaaS customers to monitor vanity domain usage.

Observability must match enforcement granularity. Always verify execution order using the [phases](https://developers.cloudflare.com/ruleset-engine/reference/phases-list/) list.

## 4. Developer Platform (Customization)

Platform providers offering custom code execution environments for tenants must enforce strict sandboxing. Use **[Workers for Platforms](https://developers.cloudflare.com/cloudflare-for-platforms/workers-for-platforms/) (WfP)** to isolate tenant workloads securely.

- **[Dynamic Dispatch Worker](https://developers.cloudflare.com/cloudflare-for-platforms/workers-for-platforms/get-started/dynamic-dispatch/):** Routes requests to tenant [User Workers](https://developers.cloudflare.com/cloudflare-for-platforms/workers-for-platforms/get-started/user-workers/), handles authentication, sanitizes responses.
- **[Outbound Workers](https://developers.cloudflare.com/cloudflare-for-platforms/workers-for-platforms/configuration/outbound-workers/):** Restrict and control outbound (egress) traffic to prevent data exfiltration.
- **[Rate Limiting API](https://developers.cloudflare.com/workers/runtime-apis/bindings/rate-limit/):** Enforce per-tenant or per-function throughput within Workers.
- **[Workers Analytics Engine](https://developers.cloudflare.com/analytics/analytics-engine/) (WAE):** Track tenant usage and detect anomalies. **[Alternative storage solutions](https://developers.cloudflare.com/workers/platform/storage-options/)** are available; one recommended one is [Cloudflare D1 SQLite database](https://developers.cloudflare.com/d1/platform/limits/), which supports _for millions to tens-of-millions of databases (or more) per account_.

> No tenant code executes without boundary isolation.

In terms of observability:
- [Workers Observability](https://developers.cloudflare.com/workers/observability/), which includes metrics and analytics, [Logs](https://developers.cloudflare.com/workers/observability/logs/), and more.
- Build your own with [Tail Workers](https://developers.cloudflare.com/workers/observability/logs/tail-workers/) and/or [Workers Analytics Engine (WAE)](https://developers.cloudflare.com/analytics/analytics-engine/).
- When using [Custom Domain / Routes](https://developers.cloudflare.com/workers/configuration/routing/), one also also benefits of the HTTP/S Application Services visibility.

## 5. Security Framework Summary

| Layer              | Goal                                   | Cloudflare Mechanism                             |
| ------------------ | -------------------------------------- | ------------------------------------------------ |
| Account            | Shared baseline                        | Account-level WAF, Rate Limiting, Gateway DNS    |
| Application        | Tenant differentiation                 | Metadata, JWT, API Key, Headers, Tier Logic               |
| Observability      | Tenant visibility                      | Logpush, Analytics, GraphQL                      |
| Developer Platform | Tenant execution & storage isolation | Workers for Platforms, Rate Limit API, D1 SQLite |

## **Result**

Tenant-aware security transforms Cloudflare's global edge into an adaptive programmable enforcement and execution fabric – distributed, per-tenant isolated, and inherently resilient. Shared infrastructure remains shared in design only, not in exposure.

---

## Disclaimer

Educational purposes only.

This blog post is independently created and is not affiliated with, endorsed by, or necessarily representative of the views or opinions of any organizations or services mentioned herein.

The images used in this article primarily consist of screenshots from the Cloudflare Dashboard or other publicly available materials, such as Cloudflare webinar slides.

The author of this post is not responsible for any misconfigurations, errors, or unintended consequences that may arise from implementing the guidelines or recommendations discussed herein. You assume full responsibility for any actions taken based on this content and for ensuring that configurations are appropriate for your specific environment and use case.
