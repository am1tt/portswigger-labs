## 🧪 Lab: Inconsistent Security Controls  
**Level**: Apprentice  
**Date**: June 27, 2025  
**Goal**: Bypass authorization by abusing email validation and user role assignment logic.

---

### 🧰 Tools  
- Burp Suite (Repeater)  
- Browser Developer Tools  
- Exploit Server (Email Simulation)

---

### 🧭 Steps

1. Start by analyzing the flow from login to registration using a proxy tool like Burp Suite.  
2. Attempted accessing `/admin` endpoint directly — endpoint was accessible but required an email with `@companymail` to unlock admin access.  
3. Tried registering a new user, but could not complete registration due to no access to the confirmation email.  
4. Open the provided email client and copy the **Exploit Server** link.  
5. Register a test user using the **Exploit Server** as the email address.  
6. The registration confirmation email appears in the email client; use it to activate and log in the test user.  
7. Access to `/admin` still restricted — requires `@companymail` domain.  
8. Discovered a functionality called **"Update Email"** in the user settings.  
9. Updated the email to `test@companymail.com` via the update feature.  
10. After updating the email, the admin panel became accessible.  
11. Used updated credentials to log in and **delete the `carlos` account** from the admin panel.  

✅ Lab Solved

---

### 💣 Payload (Update Email Request)


```http
POST /my-account/change-email HTTP/1.1
Host: <lab-id>.web-security-academy.net
Content-Type: application/x-www-form-urlencoded
Cookie: session=<your-session-cookie>

email=test@companymail.com
```

---

### 💡 Observations

- Email domain was the only factor for admin access — no further role-based authorization.  
- Registration confirmation emails were sent to real inboxes — vulnerable to spoofing via exploit server.  
- The **update email** functionality was not securely tied to role validation.  
- Functionality chaining (register → update email → admin access) allowed privilege escalation.  
- Encourages thinking about feature misuse — not just direct attacks.  

---

### 🔒 Remediation

- Implement strict role-based access control (RBAC) instead of relying on email domains.  
- Ensure admin privileges are not granted based solely on email patterns.  
- Validate role changes or email updates through secondary verification.  
- Prevent use of exploit servers or disposable domains in critical flows.  
- Log and monitor for suspicious privilege escalations or admin panel access.  

