## 🧪 Lab: Inconsistent Handling of an Exceptional Input  
**Level**: Practitioner  
**Date**: June 28, 2025  
**Goal**: Exploit improper email length handling to bypass email domain restrictions and gain admin access.

---

### 🧰 Tools  
- Burp Suite (Repeater / Intruder)  
- Exploit Server  
- Browser Dev Tools

---

### 🧭 Steps

1. Registered user `test`, but login failed due to required email verification.  
2. Found `/admin` endpoint — inaccessible without `@companymail` email.  
3. No "update email" option available.  
4. Registered another user with email `test@test.ca`, captured request in Repeater.  
5. Replaced email with exploit server payload, encoded `@` as `%40`, and sent to Intruder.  
6. Intruder attack used long local-part email payloads with increasing `A` blocks (100, 200, 300...).  
7. Application didn’t truncate email properly — allowed >255 characters before the `@`.  
8. Observed email field displaying incomplete local-part, but full domain: `@dontwannacry.com`.  
9. Calculated: `255 - 17 = 238`, used 239 `A`s to position `@dontwannacry.com` at the right offset.  
10. Final payload:  
```
AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA%40dontwannacry.com.exploit-0a8e009f0350825f806e483301b40027.exploit-server.net
```
11. Registered user `test3` with this email, verified via exploit server.  
12. Logged in as `test3` — email now ends with `@dontwannacry.com`.  
13. Accessed `/admin`, successfully deleted `carlos`.

✅ Lab Solved

---

### 💡 Observations

- App failed to truncate email input to 255 chars.  
- Allowed domain spoofing by pushing real domain beyond limit.  
- No validation on email length or domain after processing.  
- Email display was misleading, showing only trailing portion.

---

### 🔒 Remediation

- Enforce strict 254-character limit for email fields (per RFC).  
- Validate full email structure after decoding and before storage.  
- Do not base access control solely on displayed or parsed fields.  
- Log unusual email patterns and enforce stricter domain checks.  
