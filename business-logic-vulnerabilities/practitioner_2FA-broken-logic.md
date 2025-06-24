## 🧪 Lab: 2FA Broken Logic  
**Level**: Practitioner  
**Date**: June 24, 2025  
**Goal**: Exploit broken authentication logic in a two-step login process to gain access to another user's account.

---

### 🧰 Tools  
- Burp Suite Pro (Repeater / Intruder)  
- Browser Developer Tools  
- Manual session tampering  

---

### 🧭 Steps

1. Login using valid user credentials (e.g., `wiener` / `peter`).  
2. Observe the flow:  
   - `POST /login` (Username & Password)  
   - `GET /login2` (MFA Page — session assigned)  
   - `POST /login2` (Verify & MFA Code)  
3. Send `GET /login2` to Repeater and **remove session cookie** → still returns 200 → session not enforced properly.  
4. Send multiple invalid `POST /login2` with `verify=carlos` and wrong MFA → response still renders → vulnerable.  
5. Use **Intruder(Sniper)** on `POST /login2` with `verify=carlos` and a number list for `mfa=` (e.g., 0000–9999).  
6. Identify the correct MFA code via a **302 redirect** in response.  
7. Copy the **`session` value** from the successful 302 response.  
8. In DevTools, replace `wiener`'s session with `carlos`'s session.  
9. Visit `/my-account` → now logged in as `carlos`.  

✅ Lab Solved

---

### 💣 Payload (Intruder Sniper Position)

POST /login2 HTTP/1.1
...

```
verify=carlos&mfa=§0000-9999§
```

Look for response status `302 Found` indicating correct MFA.

---

### 💡 Observations

- MFA flow lacked proper session validation between steps.  
- Bruteforcing was possible due to missing account lockout.  
- Rendered responses leaked hints about authentication logic.  
- Emotionally rushed → skipped logical testing flow, led to frustration.  
- Manual session swapping is a powerful post-exploitation move.

---

### 🔒 Remediation

- Enforce strict session validation for each authentication step.  
- Rate-limit and lock accounts on repeated failed attempts.  
- Ensure server verifies MFA only for the correct session context.  
- Sanitize and hash sensitive params (e.g., session ID, verify user).  
- Log and monitor unusual login behavior for anomaly detection.