## 🕵️ Lab: Authentication Bypass via Information Disclosure  
**Level**: Apprentice  
**Date**: July 10, 2025  
**Goal**: Gain unauthorized access to the admin panel by leveraging exposed headers and insecure endpoint handling.

---

### 🧰 Tools  
- Burp Suite  
- Repeater  
- Match and Replace  

---

### 🧭 Steps  

1. Observed the **login flow** using Burp Suite. Credentials were embedded in the request and authenticated normally.  
2. Guessed and accessed the `/admin` endpoint.  
   - The page displayed a message indicating admin-only access was required.  
3. Sent the following request to **Repeater** and changed the HTTP method:  
   ```
   TRACE /admin HTTP/1.1
   Host: target.site
   ```  
4. The server responded with `200 OK`, and the response included:  
   ```
   X-Custom-IP-Authorization: 115.96.217.187
   ```  
5. Opened **Proxy > Match and Replace** in Burp Suite:  
   - Added a new rule to replace request header:  
     - Header Name: `X-Custom-IP-Authorization`  
     - Header Value: `127.0.0.1`  
   - This loopback address tricks the backend into thinking the request is coming from localhost.  
6. Clicked "OK" and navigated back to the home page.  
   - The **Admin Panel** now became visible in the UI.  
7. Accessed the panel and deleted the user `carlos`.  
   ✅ **Lab solved successfully**.

---

### 🧪 Payload  
```
TRACE /admin HTTP/1.1  
Host: your-lab-target  
```

Match & Replace Rule:  
```
Header Name: X-Custom-IP-Authorization  
Header Value: 127.0.0.1
```

---

✅ Lab Solved  

---

### 💡 Observations  

- This lab showcased the impact of insecure HTTP methods like TRACE when left enabled in production.  
- Using Burp’s **Match and Replace** is a powerful technique to simulate header tampering.  
- Loopback addresses (127.0.0.1) are often trusted implicitly by server logic — a common mistake if not properly validated.  
- Identifying hidden endpoints and misconfigured behavior can lead to complete authentication bypass.

---

### 🔒 Remediation  

- Disable unused HTTP methods like TRACE, OPTIONS, and TRACK in production environments.  
- Restrict access to administrative endpoints using robust authentication and strict IP whitelisting where appropriate.  
- Never rely on headers alone for access control, especially those like `X-Custom-IP-Authorization`.  
- Implement server-side validation for session and user roles, independent of any spoofable request data.  
- Ensure internal-only endpoints (e.g., `/admin`) are not exposed or guessable. Use obfuscation **and** proper authorization controls.  
- Employ CSRF protections and revalidate sessions periodically for high-privilege operations.  
- Regularly scan server configurations to detect unnecessary or dangerous HTTP methods and headers.

