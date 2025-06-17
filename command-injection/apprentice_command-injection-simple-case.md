## 🧪 Lab: OS Command Injection - Simple Case  
**Level**: Apprentice  
**Date**: June 17, 2025  
**Goal**: Exploit a basic OS command injection vulnerability to run arbitrary system commands.

---

### 🧰 Tools  
- Burp Suite (Repeater / Interceptor)  

---

### 🧭 Steps

1. **Analyze the application flow**  
   - Intercept and analyze the entire application  
   - Identify the 'Check Stock' functionality with:
     ```  
     productId & storeId  
     ```

2. **Initiate a basic request**  
   - Select product and store, click **Check Stock**  
   - Observe that a response shows reflected stock count  
   - Possible injection point in `storeId` parameter

3. **Attempt simple injection**  
   - Try appending a command:
     ```
     productId=2&storeId=1&whoami
     ```
   - ❌ No command executed

4. **Successful injection using pipe operator**  
   - Payload that worked:
     ```
     productId=2&storeId=1|whoami
     ```
   - ✅ Command executed; server returns `whoami` output

---

### 💣 Payload

```
productId=2&storeId=1|whoami
```

---

### 💡 Observations
- Using `&` didn’t work as expected — server likely ignores or escapes it  
- `|` allowed chaining commands, leading to code execution  
- Reflected inputs are a red flag for possible injection vectors  
- Intercepting and replaying requests in Burp helps in identifying behaviors

---

### 🔒 Remediation
- Never concatenate user input in system commands  
- Use language-specific secure system call methods  
- Validate and sanitize all input (especially IDs and parameters)  
- Implement strict allow-lists for inputs and avoid exposing internal logic  
