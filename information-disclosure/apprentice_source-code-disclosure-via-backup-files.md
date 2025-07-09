## 🕵️ Lab: Source Code Disclosure via Backup Files  
**Level**: Apprentice  
**Date**: July 9, 2025  
**Goal**: Identify exposed backup files containing source code and extract sensitive credentials from them.

---

### 🧰 Tools  
- Burp Suite  
- Browser DevTools  

---

### 🧭 Steps  

1. Explored the request/response flow using **Burp Suite**, starting from `/` and visiting key paths like `/items`.  
2. Opened the browser’s source code view using `Ctrl + U` to look for embedded links or hidden paths.  
3. Found a clue indicating the presence of a hidden directory.  
   - Navigated manually to:  
     ```
     /backup
     ```  
4. The directory listing revealed a backup file:  
   ```
   ProductTemplate.java.bak
   ```  
5. Opened the `.bak` file directly in the browser.  
   - Located a **hardcoded password** stored as a string in the Java source code.  
6. Copied the password and submitted it — ✅ **Lab successfully completed**.

---

### 🧪 Payload  
```
GET /backup/Producttemplate.java.bak HTTP/1.1  
Host: TARGET_DOMAIN  
Cookie: session=xyz123  
```

---

✅ Lab Solved  

---

### 💡 Observations  

- Backup and source code files were directly accessible from the browser — a **severe misconfiguration**.  
- These files can leak critical internals like **database credentials, API keys**, or authentication logic.  
- Learned the importance of checking for **common dev file extensions** like `.bak`, `.old`, `.java`, and `.zip`.  
- Highlighted the value of combining proxy flow analysis with **manual source inspection**.  
- Improved at **thinking systematically**, avoiding rushed trial-and-error in favor of reasoned exploration.

---

### 🔒 Remediation  

- 🚫 Do not store `.bak`, `.old`, or source code archives in publicly accessible directories.  
- 🧬 Move sensitive values like passwords into environment variables (`.env`) and exclude them from deployment.  
- 🔐 Update `.gitignore` or CI/CD rules to exclude dev/backup files from production releases.  
- 🧹 Regularly scan and clean up unused files from staging or production servers.  
- 🔍 Use web application firewalls (WAFs) to detect and block requests to suspicious paths like `/backup`.  
- 🛡️ Implement **least privilege access** and deny directory listing at the web server level (e.g., via `.htaccess`, nginx rules).  
