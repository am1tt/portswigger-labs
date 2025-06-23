## 🧪 Lab: Excessive Trust in Client-Side Controls  
**Level**: Apprentice  
**Date**: June 23, 2025  
**Goal**: Exploit a business logic flaw where price values are trusted from the client side during item purchase.

---

### 🧰 Tools  
- Burp Suite (Interceptor / Repeater)  
- Manual parameter tampering  
- Browser cart interaction  
---

### 🧭 Steps

1. **Login** using credentials provided or default user (if applicable).  
2. Browse to the vulnerable item page (the one mentioned to purchase).  
3. **Intercept the request when clicking "Add to cart"** using Burp Interceptor.  
4. Notice the following parameter in the request:
```
POST /cart
&price=12300
```
5. Change the price to a lower amount that you have credit for:
```
&price=20
```
6. Forward the modified request.  
7. Go to the **cart page**, verify the price is updated.  
8. Click **"Checkout"** to complete the purchase at the tampered price.

✅ Lab Solved

---

### 💣 Payload

```
&price=20
```

---

### 💡 Observations

- The server blindly trusted client-side `price` input.
- Missed intercepting "Add to Cart" initially caused delay in solving.
- Reinforces the importance of analyzing every interaction flow.
- Business Logic vulnerabilities can often hide in expected features.

---

### 🔒 Remediation

- Never trust price or logic parameters from the client side.
- Enforce price validation strictly on the server.
- Tie product prices to server-side database values.
- Log and alert unusual client-side input discrepancies.

