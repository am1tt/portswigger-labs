## 🛡️ Lab: URL-based access control can be circumvented  
**Level**: Practitioner  
**Date**: July 5, 2025  
**Goal**: Gain access to a protected admin page by bypassing frontend access controls using request header manipulation.

---

### 🧰 Tools  
- Burp Suite  
- Repeater  
- Custom HTTP Headers

---

### 🧭 Steps  

1. Tried to access the **`/admin`** endpoint directly.  
   - Received a generic denial response — likely filtered by a **front-end** system.  

2. Sent the request to **Burp Repeater** for manual testing.  
   - Changed the request path to `/`  
   - Added a new header:
     ```
     X-Original-URL: /invalid
     ```
   - Received a `404 Not Found` instead of a frontend block — confirming that the backend uses `X-Original-URL` for routing.

3. Changed the header to:
   ```
   X-Original-URL: /admin
   ```
   - Successfully accessed the admin panel despite being unauthorized!

4. To complete the lab:
   - Changed the query string to:
     ```
     /?username=carlos
     ```
   - Updated the header again:
     ```
     X-Original-URL: /admin/delete
     ```

✅ User `carlos` deleted — **Lab Solved!**

---

### 🧪 Payload Summary  

**Modified Request Example**:
```
GET /?username=carlos HTTP/1.1  
Host: YOUR-LAB-ID.web-security-academy.net  
X-Original-URL: /admin/delete  
Cookie: session=your-valid-session  
```

---

### 🧠 What I Learned  

- How **URL rewriting** or **reverse proxy headers** like `X-Original-URL` can be abused if not securely handled.  
- The danger of frontend-only access control enforcement.  
- Why request headers should always be **validated and sanitized** on the backend.

---

### 🔐 Mitigation & Prevention  

- Never trust headers like `X-Original-URL`, `X-Rewrite-URL`, or `X-Forwarded-*` unless strictly controlled.  
- Perform access control checks **after resolving** the effective URL on the server.  
- Implement RBAC or privilege checks based on authenticated session/user data, **not just path-based routing**.  
- Sanitize and log suspicious header manipulation attempts.  
- Use security-aware reverse proxies that prevent header spoofing.

---
