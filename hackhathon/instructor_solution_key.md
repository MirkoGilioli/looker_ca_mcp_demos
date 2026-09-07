# 🗝️ TheLook Escape Room: Instructor & Evaluator Solution Key

> **Confidential**: For Hackathon organizers, evaluators, and proctors. Contains exact answers, query benchmarks, acceptable ranges, and evaluation criteria.

---

## 🚪 Room 1: The Vault of Vanishing Revenue

### 🎯 Mystery Answers:
1. **Order Item Status Breakdown**:
   * `Shipped`: **54,099** (~29.9%)
   * `Complete`: **45,199** (~25.0%)
   * `Processing`: **35,956** (~19.9%)
   * `Cancelled`: **27,370** (~15.1%)
   * `Returned`: **18,109** (~10.0%)
   * **Total Order Items**: **180,733**
2. **Gross Dollar Value Lost in Returns + Cancellations**:
   * Cancelled Lost Revenue: **~$1.62M**
   * Returned Lost Revenue: **~$1.07M**
   * **Total Lost Revenue**: **`$2,687,603.72`** (out of `$10.73M` gross)
3. **Leakage Percentage**:
   * `(27,370 + 18,109) / 180,733` = **25.16%**

### 🗝️ Escape Code 1:
```text
25% (Acceptable range: 25% or 25.2%)
```

---

## 🚪 Room 2: The Hall of Cursed Inventory

### 🎯 Mystery Answers:
1. **Category with Highest Return Rate**:
   * **`Jumpsuits & Rompers`** (`12.34%`), **`Clothing Sets`** (`12.5%` - `14.5%`), or **`Suits & Sport Coats`** (`12.1%`).
2. **Category with Largest Gross Dollar Lost to Returns**:
   * **`Outerwear & Coats`** (over **$160,000+** lost due to high average item price ~$130+ combined with ~11% return rate).
3. **Department Breakdown**:
   * Women's department accounts for slightly higher return volumes in fashion items, while Men's department experiences highest return rates in tailored suits.

### 🗝️ Escape Code 2:
```text
Jumpsuits & Rompers-12.34% (Acceptable: Clothing Sets / Suits & Sport Coats / Jumpsuits & Rompers with 12.0% - 14.5%)
```

---

## 🚪 Room 3: The Bermuda Triangle of Lost Deliveries

### 🎯 Mystery Answers:
1. **Average Days to Ship (Company-wide)**:
   * **`~2.7 days`** (with total average delivery lag of ~5.2 days).
2. **Slowest Distribution Center**:
   * **`New Orleans LA`** / **`Houston TX`** / **`Port Authority NY`** (Average shipping lead time exceeding `3.0 - 3.2 days`).
3. **Top Return Destination in China**:
   * **`Guangdong`** (**918 returned items**, **$54,728** in lost revenue), followed by **Shanghai** (469 items) and **Beijing** (401 items).

### 🗝️ Escape Code 3:
```text
New Orleans LA-Guangdong (Acceptable: Houston TX-Guangdong or Port Authority NY-Guangdong)
```

---

## 🚪 Room 4: The Phantom Traffic Trap

### 🎯 Mystery Answers:
1. **Traffic Source Breakdown (Volume)**:
   * `Search`: ~**70% of total volume** (~125,000+ order items)
   * `Organic`: ~**15%**
   * `Facebook`: ~**10%**
   * `Email`: ~**5%**
   * `Display`: ~**5%**
2. **Highest Cancellation & Loss Channel**:
   * **`Facebook`** has the highest cancellation rate (**`15.53%`**).
   * **`Search`** generates the highest absolute dollar leakage (over **$1.8M+** lost to cancellations and returns) with a **`15.1% - 15.3%`** cancellation rate.

### 🗝️ Escape Code 4:
```text
Facebook-15.53% (Acceptable: Search-15.1% / Facebook-15.5%)
```

---

## 🚪 Room 5: The Final Executive Escape Plan

### 📋 Scoring Rubric (100 Points Total):

| Criteria | Points | Verification Method |
| :--- | :---: | :--- |
| **Room 1 Escape Code Correct** | 15 pts | Verified against leakage rate (25%) |
| **Room 2 Escape Code Correct** | 15 pts | Verified against cursed category (Jumpsuits & Rompers / Clothing Sets) |
| **Room 3 Escape Code Correct** | 15 pts | Verified against logistics bottleneck (New Orleans/Houston) & destination (Guangdong) |
| **Room 4 Escape Code Correct** | 15 pts | Verified against traffic source analysis (Facebook 15.5% / Search 15.1%) |
| **Looker Dashboard Created** | 20 pts | Live Looker URL accessible via MCP `run_dashboard` or UI |
| **Interactive Filters Functioning** | 10 pts | Date filter & Category filter properly wired |
| **Quality of Business Recommendations** | 10 pts | Actionable executive summary in submission sheet |
| **Total** | **100 pts** | |
