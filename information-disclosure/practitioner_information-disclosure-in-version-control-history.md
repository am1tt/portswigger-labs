## 🕵️ Lab: Information Disclosure in Version Control History  
**Level**: Practitioner  
**Date**: July 14, 2025  
**Goal**: Gain unauthorized access using leaked credentials discovered through exposed Git version control history.

---

### 🧰 Tools  
- Git  
- Browser  
- Wget  

---

### 🧭 Steps  

1. Navigated to the following URL to check for exposed Git metadata:  
   https://YOUR-LAB-ID.web-security-academy.net/.git/  
   ➤ Confirmed the `.git/` directory was publicly accessible — a major information disclosure.

2. Recursively downloaded the entire `.git/` repository:  
   ```
   wget -r https://YOUR-LAB-ID.web-security-academy.net/.git/
   ```

3. Entered the downloaded project folder:  
   ```
   cd YOUR-LAB-ID.web-security-academy.net
   ```

4. Viewed the Git commit history to look for sensitive info:  
   ```
   git log
   ```  
   ➤ Found commit message:  
   `Remove admin password from config`

5. Inspected the changes made in the commit:  
   ```
   git show <commit-id>
   ```  
   ➤ Revealed credential leak:  
   ```diff
   - admin_password = "supersecretpassword123"
   + admin_password = os.environ.get("ADMIN_PASSWORD")
   ```

6. Used the leaked credentials to log into the app:  
   - Username: `administrator`  
   - Password: `supersecretpassword123`  

7. Visited `/admin` panel and deleted the user `carlos`.  
   ✅ Lab successfully completed.

---

### 🧪 Payload  
No direct payload used — exploitation was performed via Git metadata access and version history analysis.

---

✅ Lab Solved  

---

### 💡 Observations  

- The `.git/` folder was left publicly accessible — a major misconfiguration.  
- Git commit history preserved sensitive credentials even after removal from the latest version.  
- This lab reinforced the value of reviewing Git logs and diffs when assessing leaked data.  
- Learned practical use of `wget`, `git log`, and `git show` in forensic-style code base investigation.

---

### 🔒 Remediation  

- 🚫 Never allow public access to `.git/` directories. Block with `.htaccess`, Nginx, or server configuration.  
- 🧹 Ensure deployment pipelines remove version control data before going live.  
- 🧾 Use `.gitignore` wisely to prevent committing sensitive data (e.g., `.env`, config files).  
- 🔐 Store secrets using environment variables or secure secrets managers — **never hardcode them**.  
- 🔍 Regularly scan public assets for secrets using tools like `truffleHog`, `git-secrets`, or GitHub’s secret scanning.  
- 📦 Always deploy from clean, compiled, or packaged builds — never directly from a Git-tracked working directory.

