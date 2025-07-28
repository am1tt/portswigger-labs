## 🛡️ Lab: User Role Controlled by Request Parameter  
**Level**: Apprentice  
**Date**: July 25, 2025  
**Goal**: Bypass role-based access control by manipulating a client-side cookie and gaining unauthorized access to the admin panel.

---

### 🧰 Tools Used  
- Burp Suite (Proxy, Intercept)  
- Web Browser  

---

### 🧭 Steps  

1 . Navigated to:  
   https://YOUR-LAB-ID.web-security-academy.net/admin  
   → Received a message: "Only admins can access this page"

2. Visited the login page and signed in with test credentials:  
   Username: wiener  
   Password: peter  

3. In Burp Suite, turned ON "Intercept Response" in the Proxy tab.

4. Submitted the login form and intercepted the HTTP response.  
   Observed the following header in the response:

   HTTP/1.1 200 OK  
   Set-Cookie: Admin=false; Secure  

5. Modified the intercepted response before forwarding it to:

   Set-Cookie: Admin=true; Secure  

6. Forwarded the modified response to the browser.

7. Reloaded`/admin` endpoint.  
   → Admin panel became accessible.

8. Clicked "Delete" next to the user `carlos`.  
   ✅ Lab successfully solved.

---


### 🧪 Raw Response Header (Before & After)

#### Original Response:
HTTP/1.1 200 OK
Content-Type: text/html
Set-Cookie: Admin=false; Secure
...

#### Modified Response (manually edited in Burp Suite):
HTTP/1.1 200 OK
Content-Type: text/html
Set-Cookie: Admin=true; Secure
...

---

### 🧠 Observations

- The lab relied on a simple cookie (`Admin=false`) to distinguish between user and admin roles.

- No server-side validation of privileges — access to the admin panel was controlled purely by the cookie value.

- Modifying the response before it reached the browser allowed full privilege escalation.

- Reinforces the concept: Never trust user-controllable data for security decisions.

---

### 🔐 Mitigation Strategies

1. Never store or trust privilege flags (like Admin=true) in client-side cookies without secure verification.

2. Use server-side session tracking mechanisms to manage user roles and permissions securely.

3. If using tokens (e.g., JWT), sign them with a strong secret and validate on every request.

4. Validate roles on the server for all protected routes like `/admin`.

5. Monitor user behavior for anomalies such as privilege escalation or suspicious cookie tampering.

6. Regularly test access controls using role-switching and session manipulation during pentesting cycles.