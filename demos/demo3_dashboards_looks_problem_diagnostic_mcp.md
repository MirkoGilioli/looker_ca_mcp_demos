# Demo 3: Dashboards, Looks & Business Diagnostic with Looker MCP

A step-by-step instructor and presenter guide for demonstrating end-to-end LookML metrics enrichment, business problem diagnosis, Looker dashboard construction, dashboard filter binding, and Look generation using the **Looker Model Context Protocol (MCP)** server.

---

## 🎯 Objective
Showcase how an AI assistant connected via Looker MCP can autonomously:
1. Conduct a deep-dive business diagnostic on an existing LookML model (`thelook_ecommerce`).
2. Identify missing financial, margin, and operational metrics and enrich the LookML view files (`update_project_file`).
3. Validate the enriched LookML model with zero errors (`validate_project`).
4. Programmatically create an executive dashboard container (`make_dashboard`).
5. Configure global interactive filters for date ranges and categories (`add_dashboard_filter`).
6. Populate the dashboard with specialized analytic tiles exposing business leakages and bottlenecks (`add_dashboard_element`).
7. Discover top return geographic destinations and save a dedicated Look (`make_look`).
8. Execute and verify the complete dashboard output (`run_dashboard`).

---

## 📋 Prerequisites & Environment Setup

### 1. Looker Instance & Project
* LookML project **`thelook_ecommerce`** configured with model file `thelook_ecommerce.model.lkml` and views in the `views/` directory.
* Active BigQuery connection **`default_bigquery_connection`**.

### 2. Antigravity MCP Configuration (`~/.gemini/config/mcp_config.json`)
Verify that the Looker MCP server is registered in your configuration:
```json
{
  "mcpServers": {
    "looker": {
      "oauth": {
        "clientId": "<YOUR_CLIENT_ID>"
      },
      "url": "https://<instance_id>.looker.app/mcp"
    }
  }
}
```

### 3. Authenticate via MCP
If the session token is expired or not authenticated, run:
```bash
/mcp auth looker
```

---

## 🚀 Step-by-Step Classroom Demo Flow

### Step 1: Deep Dive Business Diagnostic & Metric Enrichment

#### 🗣️ Instructor Talking Point:
> *"A basic LookML schema often lacks domain-specific business measures like gross profit margins, return rates, cancellation rates, and fulfillment velocity. Through MCP, an AI agent can analyze current shortcomings, update the LookML view files with production-grade business KPIs, and validate the changes instantly."*

#### 💬 Chat Prompt to Give the AI:
```text
Conduct a deep dive analysis of thelook_ecommerce model in such a way I discover all the possible problems with my business. The analysis will produce a detailed dashboard. In order to come up with the result you might create new dimensions and measures that are missing.
```

#### ⚙️ Under the Hood (MCP Tools Triggered):
1. **`get_project_file`** (`{"project_id": "thelook_ecommerce", "file_path": "views/order_items.view.lkml"}`) – Inspects existing dimensions and measures.
2. **`dev_mode`** (`{"devMode": true}`) – Ensures the session is in Development Mode.
3. **`update_project_file`** (`views/order_items.view.lkml` & `views/inventory_items.view.lkml`) – Adds missing KPIs:
   * **Financial Measures**: `total_gross_revenue`, `total_revenue`, `total_lost_revenue`, `total_margin`, `margin_rate`
   * **Leakage & Quality Measures**: `returned_count`, `cancelled_count`, `return_rate`, `cancellation_rate`
   * **Logistics & Velocity**: `days_to_ship`, `days_to_deliver`, `avg_days_to_ship`, `avg_days_to_deliver`
4. **`validate_project`** (`{"project_id": "thelook_ecommerce"}`) – Confirms 0 syntax or reference errors.
5. **`query`** (`{"model": "thelook_ecommerce", "explore": "order_items", "fields": [...]}`) – Executes a baseline check discovering:
   * **$2.69M Lost Revenue** from cancellations and returns (25% leakage).
   * **15.14% Cancellation Rate** and **10.02% Return Rate**.

---

### Step 2: Build the Executive Diagnostic Dashboard Container & Filters

#### 🗣️ Instructor Talking Point:
> *"Now we programmatically construct the Executive Dashboard. Looker MCP requires creating the dashboard container first, adding interactive filters second, and then binding tiles to those filters."*

#### ⚙️ Under the Hood (MCP Tools Triggered):
1. **`make_dashboard`**:
   ```json
   {
     "title": "TheLook eCommerce: Business Health & Leakage Diagnostic",
     "description": "Executive diagnostic dashboard identifying revenue leakage, high return categories, cancellation rates, profit margin outliers, and fulfillment delays."
   }
   ```
   * Returns: `{"id": "14", "url": "https://<instance_id>.looker.app/dashboards/..."}`.

2. **`add_dashboard_filter`** (Date Range Filter):
   ```json
   {
     "dashboard_id": "14",
     "name": "date_filter",
     "title": "Order Date Range",
     "filter_type": "date_filter",
     "default_value": "3 years"
   }
   ```

3. **`add_dashboard_filter`** (Product Category Filter):
   ```json
   {
     "dashboard_id": "14",
     "name": "category_filter",
     "title": "Product Category",
     "filter_type": "field_filter",
     "model": "thelook_ecommerce",
     "explore": "order_items",
     "dimension": "products.category",
     "allow_multiple_values": true
   }
   ```

---

### Step 3: Populate Diagnostic Tiles Bound to Global Filters

#### 🗣️ Instructor Talking Point:
> *"Each dashboard element targets a specific business vulnerability: executive KPI leakage, high return product categories, monthly revenue trajectory, logistics bottlenecks by distribution center, and traffic acquisition channel quality."*

#### ⚙️ Under the Hood (MCP Tools Triggered):
* **`add_dashboard_element`** (Tile 1: Executive KPI Summary):
  * Fields: `total_gross_revenue`, `total_lost_revenue`, `cancellation_rate`, `return_rate`, `margin_rate`
  * Vis Config: `looker_single_record`
  * Bound Filters: `date_filter`, `category_filter`

* **`add_dashboard_element`** (Tile 2: Worst Return Rates by Category):
  * Fields: `products.category`, `order_items.count`, `order_items.returned_count`, `order_items.return_rate`, `order_items.total_lost_revenue`
  * Vis Config: `looker_column`
  * Bound Filters: `date_filter`, `category_filter`

* **`add_dashboard_element`** (Tile 3: Monthly Net vs Lost Revenue Trend):
  * Fields: `order_items.created_month`, `order_items.total_revenue`, `order_items.total_lost_revenue`
  * Vis Config: `looker_line` (stacked)

* **`add_dashboard_element`** (Tile 4: Fulfillment Lag by Distribution Center):
  * Fields: `distribution_centers.name`, `order_items.count`, `order_items.avg_days_to_ship`, `order_items.avg_days_to_deliver`
  * Vis Config: `looker_bar`

* **`add_dashboard_element`** (Tile 5: Traffic Source Quality & Leakage Risk):
  * Fields: `users.traffic_source`, `order_items.count`, `order_items.cancellation_rate`, `order_items.return_rate`, `order_items.total_lost_revenue`
  * Vis Config: `looker_grid`

---

### Step 4: Geographic Analysis & Creating a Saved Look

#### 🗣️ Instructor Talking Point:
> *"When exploring specific root causes—like where returned products are going—we can run ad-hoc queries, save the exploration as a persistent Look, and seamlessly add it as a new tile to our existing dashboard."*

#### 💬 Chat Prompt to Give the AI:
```text
Find the final destinations of the products being returned the most and add another Look at the existing dashboard.
```

#### ⚙️ Under the Hood (MCP Tools Triggered):
1. **`query`** (`{"model": "thelook_ecommerce", "explore": "order_items", "fields": ["users.country", "users.state", "order_items.returned_count", ...], "sorts": ["order_items.returned_count desc"]}`)
   * Identifies top return destinations: Guangdong (China), England (UK), California (USA), Shanghai (China), Texas (USA).
2. **`make_look`**:
   ```json
   {
     "title": "Top Return Destinations by Country and State",
     "description": "Geographic distribution of customer destinations experiencing the highest volume and rate of returned merchandise.",
     "model": "thelook_ecommerce",
     "explore": "order_items",
     "fields": ["users.country", "users.state", "order_items.returned_count", "order_items.return_rate", "order_items.total_lost_revenue"],
     "sorts": ["order_items.returned_count desc"],
     "vis_config": {"type": "looker_grid", "show_value_labels": true}
   }
   ```
   * Returns: `{"id": "1", "short_url": "https://<instance_id>.looker.app/looks/1"}`.
3. **`add_dashboard_element`** (Tile 6: Top Return Destinations):
   * Adds the geographic analysis tile to Dashboard `14` bound to `date_filter` and `category_filter`.
4. **`run_dashboard`** (`{"dashboard_id": "14"}`) – Executes and verifies all 6 dashboard queries simultaneously.

---

## 📊 Summary of Created Artifacts & Live URLs

| Artifact Type | Name / ID | URL / Access Link |
| :--- | :--- | :--- |
| **Interactive Dashboard** | `TheLook eCommerce: Business Health & Leakage Diagnostic` (ID: `14`) | [View Diagnostic Dashboard](https://c88b52e2-ebce-4d35-8f65-836444b662ba.looker.app/dashboards/2YsuUALCkx36AL8JCHEZWi) |
| **Saved Look** | `Top Return Destinations by Country and State` (ID: `1`) | [View Saved Look 1](https://c88b52e2-ebce-4d35-8f65-836444b662ba.looker.app/looks/1) |
| **Enriched View Files** | `views/order_items.view.lkml`, `views/inventory_items.view.lkml` | [Looker IDE - thelook_ecommerce](https://c88b52e2-ebce-4d35-8f65-836444b662ba.looker.app/projects/thelook_ecommerce) |
| **Validation Status** | ✅ **Passed (0 syntax or reference errors)** | Verified via `validate_project` |

---

## 🛠️ MCP Tool Reference Summary (Section 3)

| MCP Tool | Purpose | Key Parameters |
| :--- | :--- | :--- |
| **`make_dashboard`** | Creates an empty dashboard container | `title`, `description`, `folder` |
| **`add_dashboard_filter`** | Adds interactive dashboard-level filters | `dashboard_id`, `name`, `title`, `filter_type`, `dimension` |
| **`add_dashboard_element`**| Adds visualization tiles connected to filters | `dashboard_id`, `model`, `explore`, `fields`, `vis_config`, `dashboard_filters` |
| **`make_look`** | Creates and saves a standalone Look | `title`, `model`, `explore`, `fields`, `vis_config`, `sorts` |
| **`run_dashboard`** | Executes all queries across dashboard tiles | `dashboard_id` |
| **`run_look`** | Executes the saved query in a Look | `look_id` |
