## 🕵️ Lab: Unprotected Admin Functionality with Unpredictable URL  
**Level**: Apprentice  
**Date**: July 25, 2025  
**Goal**: Access an undisclosed admin panel by discovering its unpredictable location in the client-side code and delete the user `carlos`.

---

### 🧰 Tools  
- Burp Suite  
- Browser Developer Tools  

---

### 🧭 Steps  

1. Visited the **lab's home page**.  
2. Opened the page source using `Ctrl + U` or **Inspect Element**.  
3. Found a snippet of **JavaScript** that disclosed the admin panel URL:  
   ```js
   // Found in source
   var isAdmin = false;
   var adminPanel = "/admin-xgo1xk";
   ```  
4. Navigated to the admin panel directly:  
   ```
   https://0a83002203cef21480707116001d0032.web-security-academy.net/admin-xgo1xk
   ```  
5. Loaded successfully — no authentication required.  
6. Located the "Delete" button next to user `carlos`.  
7. Clicked delete — ✅ **Lab solved successfully**.

---

### 🧪 Payload  
```
GET /admin-xgo1xk HTTP/1.1  
Host: 0a83002203cef21480707116001d0032.web-security-academy.net  
```

---

✅ Lab Solved  

---

### 💡 Observations  

- Even unpredictable URLs offer no real security — **security through obscurity is not security**.  
- Client-side code (like JavaScript) can leak sensitive paths and logic, making it essential to inspect the frontend thoroughly.  
- Admin functionality must always be protected by robust **authentication and authorization**, regardless of its URL.  
- This lab highlights **broken access control** due to reliance on hidden endpoints instead of proper validation.

---

### 🔒 Remediation  

- 🛑 Do **not rely on hidden or obscure URLs** as a protection mechanism.  
- 🔐 Enforce **server-side access control** for all admin or sensitive routes.  
- 🧹 Avoid exposing admin panel paths or logic in JavaScript or client-facing assets.  
- 🧪 Regularly audit your front-end code for unintentional disclosures (admin links, tokens, or logic hints).  
- 🚫 Use proper role-based authentication, session validation, and CSRF tokens for all critical operations.  
- 👁️ Employ penetration testing and source code review to identify such logic leaks before going live.

```  

Let me know if you’d like the matching LinkedIn post or GitHub front-matter block for this!
