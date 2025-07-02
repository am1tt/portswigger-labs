## 🧪 Lab: Insufficient Workflow Validation  
**Level**: Practitioner  
**Date**: July 2, 2025  
**Goal**: Abuse improper validation of payment flow to bypass insufficient funds logic and place an order for a high-value item.

---

### 🧰 Tools  
- Burp Suite  
- Repeater  

---

### 🧭 Steps  

1. Logged in with provided credentials and observed the full app flow using Burp Proxy / HTTP history.  
2. Added the leather jacket to the cart and intercepted the `/cart` POST request.  
3. Tried negative quantity tampering — confirmed the bug had been patched.  
4. Sent a null payload directly to the `/cart` endpoint to test for unexpected behavior — no negative pricing was observed.  
5. Switched strategy — bought a **cheap item** first to understand the successful purchase flow.  
6. Sent the `GET /cart/order-confirmation?order-confirmed=true` to Repeater.  
   - Observed this endpoint was hit upon successful purchase — noted for later use.  
7. Attempted to purchase the **leather jacket** — received `cart?err=insufficient_funds`.  
8. Sent that same failed GET request to Repeater, but replaced it with:  

---

### 💣 Payload  

```
GET /cart/order-confirmation?order-confirmed=true HTTP/1.1
Host: vulnerable-app.com
Cookie: session=abc123
```


---

9. Server returned `200 OK`, indicating confirmation success — lab solved without actually having enough funds.  

✅ Lab Solved

---

### 💡 Observations  

- Flow testing led to discovering a **logic flaw in the order confirmation** process.  
- The endpoint `/cart/order-confirmation` lacked proper server-side validation.  
- No inventory or transaction validation occurred server-side — only client-side checks.  
- Reused a **successful path from a cheap item purchase** to bypass restrictions on a costly item.  
- A simple query param like `order-confirmed=true` shouldn't control transaction state alone.

---

### 🔒 Remediation  

- Hash and sanitize all sensitive endpoints.  
- Avoid exposing direct confirmation states in query strings.  
- Implement CSRF protection and session-bound validation for transactional endpoints.  
- Ensure the backend revalidates the cart, balance, and inventory before completing an order.  
