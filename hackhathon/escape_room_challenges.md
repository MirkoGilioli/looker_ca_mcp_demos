# 🏰 TheLook Escape Room: Participant Challenge Guide

> **Mission Objective**: Interrogate the Looker semantic model using natural language prompts with the Looker Conversational Agent to solve 5 thematic mysteries and collect all 5 Escape Passcodes.

---

## 🚪 Room 1: The Vault of Vanishing Revenue
### 📜 Story & Mystery
The finance department reported gross revenues exceeding **$10 Million**. However, the actual bank balance is millions of dollars short! The CFO suspects that orders are failing between placement and realization.

### ❓ Investigation Riddles:
1. What is the breakdown of order items across each distinct order status (`order_items.status`)?
2. How much gross dollar value is locked in **Cancelled** and **Returned** orders combined?
3. What percentage of total orders placed are never successfully completed?

### 🗝️ Escape Code 1:
Calculate the **Combined Leakage Percentage** (Cancelled + Returned order items as a % of total order items placed), rounded to the nearest whole percentage (e.g. `25%`).

> 💡 **Prompt Strategy Hints:**
> * *"Analyze the count and percentage of order items grouped by status in the order_items explore."*
> * *"What is the sum of sale price for orders that have status Cancelled or Returned?"*

---

## 🚪 Room 2: The Hall of Cursed Inventory
### 📜 Story & Mystery
Warehouse managers are drowning in return boxes! Certain product lines seem "cursed"—customers order them in high volumes, but a shocking proportion get sent right back, racking up reverse-logistics costs.

### ❓ Investigation Riddles:
1. Which product category has the **highest return rate** (percentage of sold items returned)?
2. Which product category accounts for the **largest gross dollar amount of lost revenue** due to returns?
3. Is there a specific department (Men vs. Women) that experiences higher return rates?

### 🗝️ Escape Code 2:
Identify the **Name of the Product Category with the #1 highest return rate** + its **Return Rate %** rounded to one decimal place (e.g. `CategoryName-14.2%`).

> 💡 **Prompt Strategy Hints:**
> * *"In the order_items explore, calculate the return rate by product category and sort by return rate descending."*
> * *"Which product categories generate the highest lost revenue from returned items?"*

---

## 🚪 Room 3: The Bermuda Triangle of Lost Deliveries
### 📜 Story & Mystery
Customer support is overwhelmed with angry tickets asking *"Where is my package?!"* Rumor has it that certain distribution centers are taking days longer to process and dispatch shipments, causing buyers to cancel in frustration before goods even leave the facility.

### ❓ Investigation Riddles:
1. What is the average number of days taken to ship an order (`days_to_ship` / `avg_days_to_ship`) across the entire company?
2. Which **Distribution Center** has the slowest average fulfillment lead time?
3. Which customer destination country and state receives the highest total volume of returns?

### 🗝️ Escape Code 3:
Enter the **Name of the Slowest Distribution Center** + the **Top Returned Destination State in China** (e.g. `DCName-Guangdong`).

> 💡 **Prompt Strategy Hints:**
> * *"What is the average days to ship grouped by distribution center name in order_items?"*
> * *"Show me the top 5 destination states with the highest returned item count in China."*

---

## 🚪 Room 4: The Phantom Traffic Trap
### 📜 Story & Mystery
The marketing team spent $500,000 on digital acquisition campaigns last quarter, boasting about tens of thousands of new user signups. But the executive team suspects that some acquisition channels are delivering low-intent "phantom" traffic that cancels before delivery.

### ❓ Investigation Riddles:
1. How many users were acquired through each `traffic_source` (Search, Organic, Facebook, Email, Display)?
2. Which acquisition channel has the **highest cancellation rate**?
3. Which traffic source is responsible for the largest absolute dollar amount of cancelled order items?

### 🗝️ Escape Code 4:
Identify the **Traffic Source with the highest total lost revenue** + its **Cancellation Rate %** (e.g. `Search-15.3%`).

> 💡 **Prompt Strategy Hints:**
> * *"Compare the traffic sources in the users view by total order count, cancellation rate, and lost revenue."*

---

## 🚪 Room 5: The Final Executive Escape Plan
### 📜 Story & Mystery
The CEO and Board of Directors are meeting in 15 minutes. To permanently solve the crisis and escape the hackathon room, your team must instruct the Looker Conversational Agent to assemble all your discoveries into a **Live Executive Looker Dashboard**.

### 📋 Dashboard Deliverables to Request:
1. **Title**: `TheLook eCommerce: Crisis & Leakage Diagnostic`
2. **Filters**:
   * Global Date Filter (`order_items.created_date`)
   * Product Category Filter (`products.category`)
3. **Tiles to Include**:
   * **Tile 1**: Executive KPI Summary (Gross Revenue, Lost Revenue, Cancellation %, Return %).
   * **Tile 2**: Worst Return Rate Categories (Bar/Column chart).
   * **Tile 3**: Distribution Center Shipping Lag Bottlenecks.
   * **Tile 4**: Traffic Source Cancellation & Quality Comparison.
   * **Tile 5**: Top Return Geographic Destinations.

### 🗝️ Master Escape Code:
Provide the **Live URL of your created Looker Dashboard**!
