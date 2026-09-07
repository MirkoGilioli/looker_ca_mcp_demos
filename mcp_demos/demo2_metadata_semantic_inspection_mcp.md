# Demo 2: LookML Metadata & Semantic Model Inspection with Looker MCP

A step-by-step instructor and presenter guide for demonstrating automated Looker semantic model discovery, model/explore hierarchy inspection, dimension and measure discovery, and field value suggestions using the **Looker Model Context Protocol (MCP)** server.

---

## 🎯 Objective
Showcase how an AI assistant connected via Looker MCP can autonomously:
1. Discover all accessible LookML models across the Looker instance (`get_models`).
2. Explore available subject-area Explores within a selected model (`get_explores`).
3. Introspect dimensions, dimension groups, data types, and primary keys exposed by an explore (`get_dimensions`).
4. Inspect aggregations, measures, and drill fields defined across joined views (`get_measures`).
5. Retrieve distinct field value suggestions dynamically to assist filter construction and data validation (`get_field_value_suggestions`).

---

## 📋 Prerequisites & Environment Setup

### 1. Looker Instance & Project
* LookML project **`thelook_ecommerce`** configured with model file `thelook_ecommerce.model.lkml` and explores `order_items` & `events`.
* BigQuery connection **`default_bigquery_connection`** active.

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
If the session token is expired or not authenticated, run the slash command in the chat:
```bash
/mcp auth looker
```

---

## 🚀 Step-by-Step Classroom Demo Flow

### Step 1: Discover Available LookML Models

#### 🗣️ Instructor Talking Point:
> *"When exploring a Looker instance, an AI agent does not need to guess project names or crawl raw files. It uses MCP discovery tools to introspect available semantic models and their underlying connection mapping."*

#### 💬 Chat Prompt to Give the AI:
```text
List all available LookML models on this Looker instance.
```

#### ⚙️ Under the Hood (MCP Tool Triggered):
* **`get_models`** (`{}`)
  * Output includes:
    * `thelook_ecommerce` (Project: `thelook_ecommerce`, Connection: `default_bigquery_connection`)
    * `sample_thelook_ecommerce` (`basic_ecomm`, `intermediate_ecomm`, `advanced_ecomm`, `super_advanced_ecomm`)
    * `marketplace_extension_api_explorer`

---

### Step 2: Discover Explores in a Model

#### 🗣️ Instructor Talking Point:
> *"Now we target a specific model (`thelook_ecommerce`) to discover the business subject areas (Explores) exposed to end users and data analysts."*

#### 💬 Chat Prompt to Give the AI:
```text
What explores are available in the "thelook_ecommerce" model?
```

#### ⚙️ Under the Hood (MCP Tool Triggered):
* **`get_explores`** (`{"model": "thelook_ecommerce"}`)
  * Returns:
    * `order_items` (Label: "Order Items", Group: "Thelook Ecommerce")
    * `events` (Label: "Events", Group: "Thelook Ecommerce")

---

### Step 3: Inspect Dimensions & Attributes

#### 🗣️ Instructor Talking Point:
> *"Explores join multiple views together. MCP lets us discover all dimensions, timeframes, and data types exposed across the joined entities (e.g. users, products, distribution centers, and orders) in one call."*

#### 💬 Chat Prompt to Give the AI:
```text
Inspect the "order_items" explore in the "thelook_ecommerce" model and list the key dimensions available from the joined views (orders, users, products, inventory_items, distribution_centers).
```

#### ⚙️ Under the Hood (MCP Tool Triggered):
* **`get_dimensions`** (`{"model": "thelook_ecommerce", "explore": "order_items"}`)
  * Discovers dimensions across joined views:
    * **`order_items`**: `id`, `sale_price`, `status`, `created_date`, `delivered_date`, `shipped_date`, `returned_date`
    * **`orders`**: `order_id`, `status`, `created_date`, `num_of_item`
    * **`users`**: `id`, `first_name`, `last_name`, `city`, `state`, `country`, `age`, `gender`
    * **`products`**: `id`, `name`, `category`, `brand`, `department`, `retail_price`, `cost`
    * **`inventory_items`**: `id`, `cost`, `product_name`, `created_date`, `sold_date`
    * **`distribution_centers`**: `id`, `name`, `latitude`, `longitude`

---

### Step 4: Discover Aggregations & Measures

#### 🗣️ Instructor Talking Point:
> *"Next, we inspect the quantitative metrics and aggregations defined on the explore. Looker MCP reveals measure names, aggregation types (e.g. `count`, `count_distinct`, `sum`), and drill fields."*

#### 💬 Chat Prompt to Give the AI:
```text
What measures and metrics are defined in the "order_items" explore?
```

#### ⚙️ Under the Hood (MCP Tool Triggered):
* **`get_measures`** (`{"model": "thelook_ecommerce", "explore": "order_items"}`)
  * Returns:
    * `order_items.count` (`type: count`)
    * `orders.count` (`type: count_distinct`)
    * `users.count` (`type: count_distinct`)
    * `products.count` (`type: count_distinct`)
    * `inventory_items.count` (`type: count_distinct`)
    * `distribution_centers.count` (`type: count_distinct`)

---

### Step 5: Introspect Field Suggestions for Dynamic Filtering

#### 🗣️ Instructor Talking Point:
> *"To ensure LLM-generated queries or filters never fail due to bad categorical values or casing typos, the agent uses `get_field_value_suggestions` to query distinct valid values directly from the database schema via Looker."*

#### 💬 Chat Prompt to Give the AI:
```text
Retrieve the possible distinct values for the "order_items.status" and "products.category" fields in the "order_items" explore.
```

#### ⚙️ Under the Hood (MCP Tool Triggered):
* **`get_field_value_suggestions`** (`{"model": "thelook_ecommerce", "explore": "order_items", "field": "order_items.status"}`)
  * Returns: `["Cancelled", "Complete", "Processing", "Returned", "Shipped"]`
* **`get_field_value_suggestions`** (`{"model": "thelook_ecommerce", "explore": "order_items", "field": "products.category"}`)
  * Returns distinct product categories (e.g., `["Accessories", "Active", "Blazers & Jackets", "Clothing Sets", "Dresses", ...]`)

---

## 📊 Summary of MCP Semantic Discovery Tools

| MCP Tool | Purpose | Typical Scenario |
| :--- | :--- | :--- |
| **`get_models`** | Lists all semantic models in the instance | Initial environment mapping and locating domain models |
| **`get_explores`** | Lists explores within a chosen model | Identifying the right subject area for a business question |
| **`get_dimensions`** | Lists all slice-and-dice dimensions & types | Choosing columns for grouping, filtering, or time-series |
| **`get_measures`** | Lists aggregated metrics and calculations | Identifying KPIs, sums, averages, and counts |
| **`get_filters`** | Lists LookML filter-only fields | Finding templated filter parameters for dynamic views |
| **`get_parameters`** | Lists user input parameters | Discovering interactive Liquid parameter controls |
| **`get_field_value_suggestions`** | Retrieves distinct values for suggestable fields | Validating filter values before constructing queries |

---

## 🔗 Verification URLs

| Resource | Link |
| :--- | :--- |
| **Looker Explore UI** | [Explore - order_items](https://c88b52e2-ebce-4d35-8f65-836444b662ba.looker.app/explore/thelook_ecommerce/order_items) |
| **Looker Model IDE** | [Model File - thelook_ecommerce.model.lkml](https://c88b52e2-ebce-4d35-8f65-836444b662ba.looker.app/projects/thelook_ecommerce) |
