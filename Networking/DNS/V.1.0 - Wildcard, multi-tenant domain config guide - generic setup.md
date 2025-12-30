---

excalidraw-plugin: parsed
tags: [excalidraw]

---
==⚠  Switch to EXCALIDRAW VIEW in the MORE OPTIONS menu of this document. ⚠== You can decompress Drawing data with the command palette: 'Decompress current Excalidraw file'. For more info check in plugin settings under 'Saving'


# Excalidraw Data

## Text Elements
V.1.0 - Wildcard, multi-tenant domain config guide - generic setup ^7Pzb7G5D

Goal: Enable each customer to access their app instance 
       via a unique subdomain like orgname.yourplatform.com.

Key Components:
    Frontend: Tenant-aware SPA (e.g., React/Vue) served via CDN or edge network.
    Backend: API gateway or reverse proxy routing to tenant-isolated services.
    Database: Per-tenant data isolation (schema, database, or row-level).
    Infrastructure: DNS, TLS, CDN, and load balancing with wildcard support.
    Identity: Tenant-scoped authentication and authorization. ^w6lIXIRh

1. High-Level Architecture Overview ^L7LmuzCw

2. Roadmap & Implementation Phases ^XywUe92F

- Choose a base domain: e.g., digitaltwin-platform.com.
- Configure wildcard DNS record: 

```
*.digitaltwin-platform.com IN  A  <your-load-balancer-IP>
*.digitaltwin-platform.com.  IN  AAAA  <your-IPv6>
```

- Use a managed DNS provider (Cloudflare, AWS Route 53, Google Cloud DNS) for reliability and DDoS protection.
✅ Best Practice: Avoid using wildcard DNS for email (MX records) — it can cause deliverability issues.


 ^juBVE7Pt

Phase 1: Domain Strategy & DNS Setup ^kOlgYF0T

Phase 2: TLS/SSL Certificate Management ^yEwgIcNj

+---------------------+
|   Customer Browser  |
|  (e.g., acme.dt.com)|
+----------+----------+
           |
           | HTTPS (TLS)
           v
+----------+----------+
|   CDN / Load Balancer|
|  (Cloudflare, ALB)  |
|  Wildcard DNS + TLS |
+----------+----------+
           |
           | HTTP + Host header
           v
+----------+----------+
|   Frontend (SPA)    |
|  Single bundle      |
|  Reads hostname     |
+----------+----------+
           |
           | API calls w/ Host header
           v
+----------+----------+
|   Backend API       |
|  Tenant Middleware  |
|  → org_id context   |
+----------+----------+
           |
           | DB queries w/ org_id
           v
+----------+----------+
|   Database          |
|  Shared, row-level  |
|  isolation          |
+---------------------+

Key:
- dt.com = digitaltwin.example.com
- All layers enforce tenant context
- Wildcard DNS + TLS at edge
- Org slug → org_id resolved early ^KpuvXvz5

%%
## Drawing
```compressed-json
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
```
%%