# 🧪 Lab: Low-level Logic Flaw

**Level:** Practitioner  
**Date:** June 26, 2025  
**Goal:** Exploit a quantity-based price calculation flaw using controlled logic flow, null payloads, and math.

---

## 🧰 Tools Used

- Burp Suite Pro  
- Intruder (null + generative payloads)  
- Resource Pool (concurrent requests = 1)  
- Repeater (manual tuning)  

---

## 🧭 Finalized Steps

### 🛠️ Initial Setup

1. Add the **item (jacket)** to the cart.  
2. **Observe:** Frontend caps quantity at **99**.  

---

### 💥 Null Payload Attack (Price Inflation)

1. Send the cart POST request to **Intruder**.  
2. Create a **Resource Pool** with only **1 concurrent request**.  
3. Set Intruder to send null payloads:

   ```text
   \0
   ```

4. This abuses logic and causes the **quantity to inflate rapidly**, pushing the **total price into negative**.

---

### 🖱️ Frontend Trigger

1. Once price is **deeply negative**, refresh page to check.  
2. Click the **“+” (add quantity)** button from the frontend.  
3. This causes the price to **decrease even further** — a **key logic flaw**.

---

### 🔁 Switch to Generative Payloads

1. Now switch Intruder to custom quantity payloads like:

   ```text
   35
   46
   58
   ```

2. Estimate how many precise requests to send:

   - **Step 1:** Divide the current negative price by jacket price (1337):

     ```text
     [Current Negative Total] / 1337 = Estimated Quantity
     ```

   - **Step 2:** Divide that result by 99 (max quantity):

     ```text
     Estimated Quantity / 99 = Safe Payload Range
     ```

3. Send **only half** the calculated range using Intruder in **Generate mode** (for safety).

---

### 🧮 Manual Tuning (Repeater)

1. Once price approaches **-1000**, stop Intruder.  
2. Move to **Repeater** and tune the quantity manually with math:

   ```text
   [Current Total Negative] / 1337 = Quantity to Add
   ```

3. Adjust and repeat until the price is around **-100**.

---

### 🎯 Final Trick — Add Another Item

1. Return to the shop and add a **low-cost item** (e.g., `productId=16`).  
2. Send its **cart POST request** to Repeater.  
3. Gradually **increase its quantity** to push the total price just **above 0** but still **under 100 credits**.

---

### ✅ Lab Solved

- With total cost **under available user credit**, proceed to **checkout** successfully.

---

## 💡 Key Learnings

- **Null payloads** and **controlled concurrency** triggered a low-level logic flaw.  
- Frontend "+" button **decreased negative price** unexpectedly.  
- **Math-based tuning** was crucial to bring price within an acceptable threshold.  
- **Intruder → Repeater hybrid workflow** provided both automation and precision.  
- **Multi-item logic chaining** enabled safe manipulation of final total.

---

## 🔒 Mitigation Strategies

- Never trust **client-side** quantity validation.  
- Enforce strict **server-side quantity caps** per item/user.  
- Disallow **negative totals** in price calculation logic.  
- Monitor for:
  - Quantity burst anomalies  
  - Unusual price drops  
  - Cart manipulation patterns
