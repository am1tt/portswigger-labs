## 🧪 Lab: Authentication Bypass via Flawed State Machine  
**Level**: Practitioner  
**Date**: July , 2025  
**Goal**: Bypass the role-selection step after login to gain unintended administrator access and delete the target user.

---

### 🧰 Tools  
- Burp Suite  
- Interceptor  

---

### 🧭 Steps  

1. Logged in as a regular user with provided credentials.  
2. Intercepted the login request (`POST /login`) via Burp and forwarded it.  
3. Observed a subsequent `GET /role-selector` request triggered after login.  
4. Dropped the `/role-selector` request in Burp's Proxy → Intercept tab.  

---

### 💣 Payload  

```
GET /role-selector HTTP/1.1  
Host: vulnerable-app.com  
Cookie: session=xyz123  
```

📌 **Action**: Drop this request using Burp Intercept. Do not forward.

---

5. Navigated directly to `/` (homepage) or `/admin`.  
6. Successfully accessed the admin panel without selecting a role — the server defaulted to `admin`.  
7. Visited `/admin/delete?username=carlos` or used the admin UI to delete the `carlos` user.  

✅ Lab Solved  

---

### 💡 Observations  

- Relearned the usefulness of the **“Drop”** feature in Burp — last used it a month ago.  
- Not all bypasses require payloads — some exploit flawed logic in flow/state handling.  
- Recognized the danger of assuming default roles without explicit confirmation.  
- Emphasized the importance of testing **simple flows first** before moving to complex attack chains.  
- Highlighted how state-machine flaws in role assignment can break access control.

---

### 🔒 Remediation  

1. Enforce access control checks on the backend, not on frontend logic or intermediate steps.  
2. Avoid assigning user roles through client-driven flows like role selectors.  
3. Validate roles and permissions server-side on every sensitive request (e.g., `/admin`).  
4. Bind roles securely to sessions after login — never allow default escalation to privileged roles.  
5. Apply the principle of **least privilege** — never default users to high-privilege roles like `admin`.  
