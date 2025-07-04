## 🔐 Lab: Authentication bypass via encryption oracle  
**Level**: Practitioner  
**Date**: July 4, 2025  
**Goal**: Exploit an encryption oracle by injecting and deleting specific blocks in an encrypted payload to escalate privileges and gain administrator access and delete the mentioned user  

---  

### 🧰 Tools  
- Burp Suite  
- Repeater  
- Decoder / Encoder  

---  

### 🧭 Steps  

1. Captured a request from the "My Account" page using Burp Suite.  
2. Located the base64-encoded encrypted blob inside a cookie or parameter.  
3. Decoded the blob using Burp Decoder → Decode as Base64.  
4. Identified a block in the decoded payload that included `administrator:`.  
5. Modified the decrypted structure by prepending 16 junk characters before `administrator:`  
   Example string:  
   `xxxxxxxxxxxxxxxxadministrator:`  
6. Re-encoded this modified plaintext into Base64.  
7. Replaced the original token with this newly encoded one in the request and sent it.  
8. Captured the server's response and extracted the new encrypted token.  
9. Decoded the new encrypted token using Burp Decoder.  
10. Deleted the **first 32 bytes** (2 full AES blocks = 2 lines in Burp Decoder).  
    - In Burp Community, manually selected the first two lines and pressed Delete.  
11. Re-encoded the result to Base64.  
12. Used the final manipulated token in the request.  
13. Server responded with admin access — lab solved.  

✅ Lab Solved  

---  

### 📦 Payload Format  

#### Modified plaintext structure before encryption:  
```
xxxxxxxxxxxxxxxxadministrator:
```

#### Byte deletion rule:  
Remove first 32 bytes after encryption (i.e., 2 AES blocks), assuming 16-byte block size.

---  

### 💡 Observations  

- This lab demonstrates how encryption oracles can be abused without decrypting the full payload.  
- Precise byte manipulation — particularly insertion and deletion aligned to block boundaries — can escalate privileges.  
- Understanding the structure of block-based encryption (e.g., AES-CBC) helps manipulate ciphertexts even without keys.  
- Manual byte deletion is a workaround in Burp Community where no GUI option exists.  

---  

### 🔒 Remediation  

- Use authenticated encryption algorithms such as AES-GCM instead of CBC/ECB.  
- Always apply HMAC or digital signature to validate payload integrity.  
- Do not encode or expose sensitive metadata (like `administrator:`) on the client side.  
- Ensure encryption tokens are tamper-proof and server-side verified.  

