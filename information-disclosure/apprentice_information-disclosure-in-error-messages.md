## 📤 Lab: Information Disclosure in Error Messages  
**Level**: Apprentice  
**Date**: July 5, 2025  
**Goal**: Trigger a server error that leaks internal framework version through an unhandled input, and extract the software stack information.

---

### 🧰 Tools  
- Burp Suite  
- Repeater  

---

### 🧭 Steps  

1. Launched the lab and reviewed the entire application flow in Burp’s Proxy and HTTP history.  
2. Visited a product listing and sent the following request to Repeater:  
   ```
   GET /product?productId=1
   ```  
3. Modified the `productId` parameter to a non-numeric value like `a` or a comment string like `--`.  
   - Example:  
     ```
     GET /product?productId="abc"
     ```  
4. Server responded with **500 Internal Server Error**.  
5. The response revealed detailed error output including the backend framework and version:  
   ```
   Apache Struts 2 2.3.31
   ```  
6. Submitted the lab — ✅ lab successfully solved.

---

### 🧪 Payload

```
GET /product?productId="abc" HTTP/1.1  
Host: vulnerable-app.com  
Cookie: session=xyz123  
```

---

✅ Lab Solved  

---

### 💡 Observations  

- The vulnerability demonstrates how improper error handling can lead to **information disclosure**, exposing internal stack details.  
- Revealing technologies like **Apache Struts**, especially version numbers, can help attackers chain known exploits.  
- It's a reminder that not all vulnerabilities come from brute force or complex logic — some are simple misconfigurations.  
- Tried multiple payloads, and learned how small variations (like a single `--`) can lead to unintended behavior.  

---

### 🔒 Remediation  

- Avoid including stack traces, framework names, or version numbers in error messages returned to users.  
- Enable **generic error handling** for user-facing environments — log details internally instead.  
- Hash and encode all sensitive parameters exposed in the URL to reduce guessability.  
- Implement **string validation and sanitization** for any user-supplied query values.  
- Ensure exception messages are encrypted, filtered, or suppressed in production environments.  
