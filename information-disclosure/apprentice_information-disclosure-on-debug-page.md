## 🕵️ Lab: Information Disclosure on Debug Page  
**Level**: Apprentice  
**Date**: July 7, 2025  
**Goal**: Discover a debug page exposing backend configuration and extract sensitive data such as secret keys.

---

### 🧰 Tools  
- Burp Suite  
- Browser DevTools  

---

### 🧭 Steps  

1. Explored the flow starting from the **home page** using Burp Suite and browser inspection tools.  
2. Tested common parameter manipulations like:  
   ```
   GET /product?productId=2'--
   ```  
   - No useful errors or disclosures observed.  
3. Right-clicked on the home page and selected **Inspect**.  
   - Discovered a hidden HTML comment:  
     ```html
     <!-- <a href=/cgi-bin/phpinfo.php>Debug</a> -->
     ```  
   - This indicates the presence of a sensitive debug file.  
4. Accessed the hidden path directly:  
   ```
   https://0a9b00120412c774800b2b2100ef00b7.web-security-academy.net/cgi-bin/phpinfo.php
   ```  
5. Used `Ctrl + F` to search for `SECRET_KEY` within the page.  
   - Found the key immediately and copied it.  
6. Submitted the key to solve the lab — ✅ **Lab successfully completed**.

---

### 🧪 Payload  
```
GET /cgi-bin/phpinfo.php HTTP/1.1  
Host: 0a9b00120412c774800b2b2100ef00b7.web-security-academy.net  
Cookie: session=xyz123  
```

---

✅ Lab Solved  

---

### 💡 Observations  

- Hidden HTML comments can disclose sensitive admin or debug paths.  
- `phpinfo.php` exposed complete backend details — a **major information leak**.  
- This vulnerability shows the importance of **manual inspection**, not just automated scanning.  
- Learned to apply previous mistakes as lessons — persistence and exploration paid off.  
- Becoming better at thinking outside the box and tracing unconventional routes.

---

### 🔒 Remediation  

- 🚫 **Never expose debug or configuration files** (`phpinfo.php`, `.env`, etc.) on production systems.  
- 🧹 **Remove all comments** in production code that reference sensitive paths or internal tools.  
- 🔐 Avoid embedding direct hyperlinks to backend routes — use secure routing and access control.  
- 🧬 Sanitize and validate all URLs, especially those involving internal resources.  
- 🔍 Review deployment pipelines to ensure **no debug files or leftover code** are pushed live.  
- 📦 Use environment-specific settings to **restrict access** to diagnostic pages (e.g., allow only internal IPs).
