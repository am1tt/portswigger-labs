```
## 🧪 Lab: Flawed Enforcement of Business Rules  
**Level**: Apprentice  
**Date**: June 29, 2025  
**Goal**: Abuse flawed coupon logic to purchase a leather jacket without paying full price by stacking hidden discount codes.

---

### 🧰 Tools  
- Burp Suite (Repeater)  
- Web Browser  
- Manual Coupon Injection

---

### 🧭 Steps

1. Navigated to the shop and added the leather jacket to the cart.  
2. Tested various coupon inputs in Burp Suite: `null`, empty, `test`, `newcust5`.  
3. Unexpectedly, applying multiple coupons increased the total — flawed logic.  
4. Explored UI further and noticed newsletter form at bottom of homepage.  
5. Briefly opened the solution tab, saw `SIGNUP30` revealed via newsletter sign-up.  
6. Closed solution, reapplied knowledge manually:  
   - Applied `SIGNUP30` for 30% off  
   - Then applied `NEWCUST5` for $5 off  
7. Stacked discount reduced price significantly — allowed order placement.  
8. Jacket purchased with almost no cost.

✅ Lab Solved

---

### 💣 Payload Sequence

```
POST /apply-coupon  
coupon=SIGNUP30
```

```
POST /apply-coupon  
coupon=NEWCUST5
```

---

### 💡 Observations

- Found multiple injection paths for coupons.  
- Missed the newsletter initially — UI detail matters.  
- Coupon stacking was not properly controlled.  
- Attempted different values before finding working codes.  
- Took initiative to retry lab after viewing partial solution.  

---

### 🔒 Remediation

- Disallow stacking of promo codes unless explicitly permitted.  
- Associate coupon usage with specific sessions or user states.  
- Enforce validation server-side to prevent unintended combinations.  
- Avoid exposing hidden promos via unauthenticated UI flows.  
- Track abnormal discount stacking or coupon abuse patterns.  
```
