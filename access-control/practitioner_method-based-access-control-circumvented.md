## 🛡️ Lab: Method-Based Access Control Can Be Circumvented  
**Level**: Practitioner  
**Date**: August 18, 2025  
**Goal**: Bypass method-based access control restrictions to perform unauthorized actions.

---

### 🧰 Tools  
- Burp Suite  
- Repeater  

---

### 🧭 Steps  

1. Logged in as **admin** with the provided credentials.  
   - Used the `/admin-roles` endpoint (via **POST**) to promote `carlos` to admin.  

2. In **Burp Proxy**, observed that `/admin-roles` requires parameters:  
   ```
   action=role_name (e.g., admin)  
   username=carlos  
   ```
   Sent the request to **Repeater**.

3. Modified the `username` to `wiener`.  
   - Received an **Unauthorized** response.

4. **Bypass Process**:  
   - Logged in as `wiener` normally and copied the **wiener session ID** from Proxy.  
   - In Repeater, took the original POST request and used **"Change Request Method"**.  
   - Burp converted the parameters to the query string of a **GET** request.  

5. Replaced the session cookie with the **wiener session**.  
   - Sent the GET request with:
     ```
     ?username=wiener&action=admin
     ```
   - Request succeeded, bypassing the method-based control.

✅ Lab Solved — Promoted `wiener` to admin via method manipulation.

---

### 🧪 Payload Summary  

**Bypass Request Example**:
```
GET /admin-roles?username=wiener&action=admin HTTP/1.1  
Host: YOUR-LAB-ID.web-security-academy.net  
Cookie: session=valid-wiener-session  
```

---

### 🧠 What I Learned  

- Method-based access control is insecure when enforced **only** by HTTP verb restrictions.  
- Attackers can switch between POST and GET to bypass controls if backend checks are incomplete.  
- Combining method tampering with **session swapping** can escalate privileges.

---

### 🔐 Mitigation & Prevention  

- Enforce **role-based access control** checks on all request methods.  
- Restrict sensitive actions to specific HTTP verbs **and** verify the user's privileges.  
- Disable unsupported HTTP methods at the server level.  
- Log unusual method usage patterns for investigation.  
- Combine authentication, authorization, and input validation for all sensitive endpoints.

---
