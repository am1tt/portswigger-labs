## 🧪 Lab: File Path Traversal - Validation of Start of Path  
**Level**: Practitioner  
**Date**: June 14, 2025  
**Goal**: Read `/etc/passwd` even when the application validates the starting path.

---

### 🧰 Tools
- Burp Suite (Repeater / Interceptor)

---

### 🧭 Steps

1. **Intercept the image request**  
   - Navigate to the product page and capture a request like:
     ```
     GET /image?filename=/var/www/images/218.png
     ```

2. **Send to Repeater**

3. **Test absolute payloads**  
   - Try:
     ```
     filename=/etc/passwd
     ```
   - Result: ❌ Missing parameter / validation fails (must begin with `/var/www/images/`)

4. **Test path traversal within expected path**  
   - Inject:
     ```
     filename=/var/www/images/../../../etc/passwd
     ```
   - This bypasses validation and successfully reads the system file.

---

### 💣 Payload
```
/var/www/images/../../../etc/passwd
```

---

### 💡 Observations
- Application requires filenames to start with `/var/www/images/`  
- Omitting this base path causes an error or blocks the request  
- Traversal must be relative to the validated base path  
- Triple sequential `../../../` successfully escapes the folder

---

### 🔒 Remediation
- Sanitize user-supplied file paths  
- Normalize paths on the backend before access  
- Avoid relying on static prefix checks for path validation  
- Hide internal file structure and avoid exposing full path in URLs  
