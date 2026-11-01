# 📡 HTTP Protocol & Web Security Reference Manual

<div align="center">

[![Domain: Web Security](https://img.shields.io/badge/Domain-Web%20Security%20%26%20AppSec-red.svg?style=for-the-badge)](https://owasp.org)
[![RFC Specifications](https://img.shields.io/badge/Standards-RFC%209110%20%2F%20HTTP%201.1%2F2%2F3-blue.svg?style=for-the-badge)](https://httpwg.org/specs/)
[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg?style=for-the-badge)](LICENSE)

<p align="center">
  <b>A comprehensive developer and security engineer's field manual detailing HTTP status codes, security response headers, request methods, and modern attack surface mitigations.</b>
</p>

</div>

---

## 📌 Overview

The **HTTP Protocol & Web Security Reference Manual** is a centralized reference covering the mechanics of web communication. It provides concise technical explanations, security implications, RFC references, and remediation guidance for modern web architectures.

---

## 📑 Table of Contents
1. [HTTP Status Code Taxonomy](#-http-status-code-taxonomy)
2. [Critical Security Headers & Defenses](#-critical-security-headers--defenses)
3. [HTTP Request Methods & Idempotency](#-http-request-methods--idempotency)
4. [Common HTTP Attack Vectors](#-common-http-attack-vectors)
5. [Auditing Commands & Field Tools](#-auditing-commands--field-tools)

---

## 🔢 HTTP Status Code Taxonomy

### `1xx` Informational
- `101 Switching Protocols`: Used when negotiating WebSocket connections via HTTP `Upgrade`.

### `2xx` Success
- `200 OK`: Request succeeded. Standard response for successful GET/POST requests.
- `201 Created`: Resource successfully created (standard REST response for `POST` / `PUT`).
- `204 No Content`: Action succeeded, but no response payload returned (common in `DELETE`).

### `3xx` Redirection
- `301 Moved Permanently`: Permanent redirect; search engines update canonical URLs.
- `302 Found` / `307 Temporary Redirect`: Temporary redirect preserving original HTTP method in 307.
- `304 Not Modified`: Cached copy remains valid (saves bandwidth with `ETag` and `If-None-Match`).

### `4xx` Client Error
- `400 Bad Request`: Malformed syntax, invalid JSON payload, or header anomalies.
- `401 Unauthorized`: Authentication required (missing or invalid `Authorization` header).
- `403 Forbidden`: Authenticated, but user lacks necessary permissions / RBAC violation.
- `404 Not Found`: Target endpoint or resource does not exist.
- `405 Method Not Allowed`: HTTP method not supported for target resource.
- `429 Too Many Requests`: Rate-limiting threshold exceeded.

### `5xx` Server Error
- `500 Internal Server Error`: Unhandled server exception.
- `502 Bad Gateway`: Reverse proxy (Nginx, Cloudflare) received an invalid upstream response.
- `503 Service Unavailable`: Server overloaded or undergoing scheduled maintenance.
- `504 Gateway Timeout`: Upstream service failed to respond within time threshold.

---

## 🛡️ Critical Security Headers & Defenses

| Header Name | Recommended Secure Configuration | Security Purpose / Defense |
| :--- | :--- | :--- |
| **`Content-Security-Policy`** | `default-src 'self'; script-src 'self'; object-src 'none';` | Mitigates Cross-Site Scripting (XSS) and data injection attacks. |
| **`Strict-Transport-Security`** | `max-age=31536000; includeSubDomains; preload` | Forces HTTPS and prevents SSL stripping / downgrade attacks. |
| **`X-Content-Type-Options`** | `nosniff` | Prevents MIME-confusion attacks and malicious file execution. |
| **`X-Frame-Options`** | `DENY` or `SAMEORIGIN` | Defends against Clickjacking and UI redressing attacks. |
| **`Referrer-Policy`** | `strict-origin-when-cross-origin` | Prevents sensitive tokens or URLs from leaking in `Referer` headers. |
| **`Permissions-Policy`** | `geolocation=(), camera=(), microphone=()` | Restricts browser hardware access (camera, microphone, sensors). |

---

## ⚡ HTTP Request Methods & Idempotency

| Method | Safe? | Idempotent? | RFC | Typical Use Case |
| :---: | :---: | :---: | :---: | :--- |
| **`GET`** | ✅ Yes | ✅ Yes | RFC 9110 | Retrieve data without side effects |
| **`HEAD`** | ✅ Yes | ✅ Yes | RFC 9110 | Same as GET, but returns headers only |
| **`POST`** | ❌ No | ❌ No | RFC 9110 | Create new entity or submit processing request |
| **`PUT`** | ❌ No | ✅ Yes | RFC 9110 | Replace or create entity idempotently |
| **`DELETE`**| ❌ No | ✅ Yes | RFC 9110 | Remove specific resource |
| **`PATCH`** | ❌ No | ❌ No | RFC 5789 | Apply partial modifications to a resource |
| **`OPTIONS`**| ✅ Yes | ✅ Yes | RFC 9110 | CORS preflight & server capability inquiry |

---

## 🚨 Common HTTP Attack Vectors

- **HTTP Request Smuggling**: Desynchronization between frontend reverse proxies and backend servers via conflicting `Content-Length` and `Transfer-Encoding` headers.
- **Host Header Injection**: Exploiting improper validation of the HTTP `Host` header to poison password resets or web cache entries.
- **CRLF Injection**: Injecting Carriage Return (`\r`) and Line Feed (`\n`) into unvalidated headers to perform response splitting and XSS.
- **Cross-Site Request Forgery (CSRF)**: Forcing state-changing actions via ambient credential transmission (mitigated via `SameSite=Strict/Lax` cookies and CSRF tokens).

---

## 🔍 Auditing Tools & Verification Commands

```bash
# Inspect response headers using cURL
curl -I https://akhilbinu.in

# Audit security headers using Nmap NSE
nmap -p 80,443 --script http-security-headers target.com

# Verify HTTP/2 & TLS handshake
curl -v --http2 https://target.com
```

---

## 📄 License

Distributed under the [MIT License](LICENSE). Authored by [Akhil Binu](https://akhilbinu.in/).
