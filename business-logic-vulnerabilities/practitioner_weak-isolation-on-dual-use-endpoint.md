## 🧪 Lab: Weak Isolation of Dual-Use Endpoints  
**Level**: Practitioner  
**Date**: July 1, 2025  
**Goal**: Exploit insufficient separation in account-related endpoints to reset the administrator’s password , access adminstrator and delete the mentioned account

---

### 🧰 Tools  
- Burp Suite  
- Repeater  

---

### 🧭 Steps

1. Logged in with provided credentials and monitored HTTP history in Burp.  
2. Identified available options to update username, email, and password via `/my-account/`.  
3. Successfully tested updating the email address — confirmed endpoint functionality.  
4. Accessed `/my-account/change-password/` and sent request with `username=administrator`, `current-password=test`, `new-password-1=test`, `new-password-2=test`.  
5. Although the password was rejected, the `username` field was updated to `administrator` — a major flaw.  
6. Crafted a new request to `/my-account/change-password/`, removed `current-password`, and sent only the following payload:

---

### 💣 Payload

```
POST /my-account/change-password HTTP/1.1  
Host: vulnerable-app.com  
Content-Type: application/x-www-form-urlencoded  
Cookie: session=abc123  

username=administrator&new-password-1=test&new-password-2=test
```

---

7. Response returned: "Password successfully updated."  
8. Logged out, logged in as `administrator`, and removed `carlos` from the admin panel.  

✅ Lab Solved

---

### 💡 Observations

- Following the logical flow of endpoints helped reveal the vulnerability.  
- Highlighted how minor changes in request structure could lead to privilege escalation.  
- Removing unnecessary fields like `current-password` was key to success.  
- Endpoint `/my-account/change-password/` lacked proper field isolation and validation.  
- Changing the username field in a password form should not be possible.

---

### 🔒 Remediation

- Hash and sanitize all exposed endpoint URLs.  
- Avoid exposing or accepting unnecessary parameters, especially in sensitive endpoints.  
- Disallow changing usernames within password or email update forms.  
- Require MFA and signed, expiring tokens tied to registered email/username for any critical      actions like password or email updates.  

