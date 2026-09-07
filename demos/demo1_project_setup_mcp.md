# Demo 1: LookML Project Setup & Automated Modeling with Looker MCP

A step-by-step instructor and presenter guide for demonstrating automated Looker project creation, schema inspection, LookML view generation, semantic modeling, and LookML validation using the **Looker Model Context Protocol (MCP)** server.

---

## 🎯 Objective
Showcase how an AI assistant connected via Looker MCP can autonomously:
1. Inspect available database connections and dataset schemas in BigQuery.
2. Initialize and configure a new LookML project (`thelook_ecommerce`).
3. Auto-generate boilerplate LookML view files directly from database tables.
4. Compose a production-ready model file with explores and relational joins.
5. Validate the project syntax with the LookML validator and run an end-to-end query test.

---

## 📋 Prerequisites & Environment Setup

### 1. Looker Instance & OAuth Client
* **Looker Instance URL**: `https://<instance_id>.looker.app/mcp`
* **OAuth Client ID**: Configured in Looker API Explorer (registered under API CORS / OAuth).

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
*A browser window will open. Click **Authorize** to log in.*

---

## 🚀 Step-by-Step Classroom Demo Flow

### Step 1: Discover Tools & Verify Database Connections

#### 🗣️ Instructor Talking Point:
> *"Before modifying or creating any Looker assets, we first switch to Development Mode and inspect our data source connections using MCP discovery tools."*

#### 💬 Chat Prompt to Give the AI:
```text
Enter development mode, check the available database connections, and list all tables within the "mydataset" dataset.
```

#### ⚙️ Under the Hood (MCP Tools Triggered):
1. **`dev_mode`** (`{"devMode": true}`) – Ensures the session is in Development Mode (`workspace_id: "dev"`).
2. **`get_connections`** (`{}`) – Identifies active connections, dialect, and multi-database support (returns `default_bigquery_connection`).
3. **`get_connection_tables`** (`{"conn": "default_bigquery_connection", "schema": "mydataset", "db": "qwiklabs-gcp-00-ed68f0c19614"}`) – Retrieves available tables:
   * `distribution_centers`
   * `events`
   * `inventory_items`
   * `order_items`
   * `orders`
   * `products`
   * `users`

---

### Step 2: Initialize the Project & Generate LookML Views

#### 🗣️ Instructor Talking Point:
> *"Now we create the project container and automatically generate view files (`.view.lkml`) directly from the table schemas in BigQuery without writing manual DDL/LookML."*

#### 💬 Chat Prompt to Give the AI:
```text
Create a new project named "thelook_ecommerce" using the "default_bigquery_connection" and auto-generate LookML views for all 7 tables in the "mydataset" dataset.
```

#### ⚙️ Under the Hood (MCP Tools Triggered):
1. **Looker Project Creation** – Initializes the LookML project `thelook_ecommerce`.
2. **`create_view_from_table`**:
   ```json
   {
     "project_id": "thelook_ecommerce",
     "connection": "default_bigquery_connection",
     "folder_name": "views",
     "tables": [
       {"schema": "mydataset", "table_name": "distribution_centers"},
       {"schema": "mydataset", "table_name": "events"},
       {"schema": "mydataset", "table_name": "inventory_items"},
       {"schema": "mydataset", "table_name": "order_items"},
       {"schema": "mydataset", "table_name": "orders"},
       {"schema": "mydataset", "table_name": "products"},
       {"schema": "mydataset", "table_name": "users"}
     ]
   }
   ```
3. **`get_project_files`** (`{"project_id": "thelook_ecommerce"}`) – Confirms all 7 view files are created in the `views/` folder.

---

### Step 3: Define Explores & Semantic Model

#### 🗣️ Instructor Talking Point:
> *"With our views generated, we now create the LookML model file (`thelook_ecommerce.model.lkml`). We specify the connection, include the views, and define relational explores with join keys."*

#### 💬 Chat Prompt to Give the AI:
```text
Update "thelook_ecommerce.model.lkml" to include all views and configure an "order_items" explore joining orders, users, inventory_items, products, and distribution_centers, as well as an "events" explore joining users.
```

#### ⚙️ Under the Hood (MCP Tool Triggered):
* **`update_project_file`**:
  ```lookml
  connection: "default_bigquery_connection"

  # include all the views
  include: "/views/**/*.view.lkml"

  datagroup: thelook_ecommerce_default_datagroup {
    max_cache_age: "1 hour"
  }

  persist_with: thelook_ecommerce_default_datagroup

  explore: order_items {
    join: orders {
      type: left_outer
      sql_on: ${order_items.order_id} = ${orders.order_id} ;;
      relationship: many_to_one
    }

    join: users {
      type: left_outer
      sql_on: ${order_items.user_id} = ${users.id} ;;
      relationship: many_to_one
    }

    join: inventory_items {
      type: left_outer
      sql_on: ${order_items.inventory_item_id} = ${inventory_items.id} ;;
      relationship: many_to_one
    }

    join: products {
      type: left_outer
      sql_on: ${order_items.product_id} = ${products.id} ;;
      relationship: many_to_one
    }

    join: distribution_centers {
      type: left_outer
      sql_on: ${products.distribution_center_id} = ${distribution_centers.id} ;;
      relationship: many_to_one
    }
  }

  explore: events {
    join: users {
      type: left_outer
      sql_on: ${events.user_id} = ${users.id} ;;
      relationship: many_to_one
    }
  }

  explore: users {}
  explore: products {}
  ```

---

### Step 4: Validate LookML & Execute End-to-End Query

#### 🗣️ Instructor Talking Point:
> *"To ensure code quality and prevent runtime errors, we run the LookML validator directly via MCP. Once validated, we execute a test query on the newly defined explore."*

#### 💬 Chat Prompt to Give the AI:
```text
Validate the "thelook_ecommerce" project to ensure there are zero errors, then run a test query on the "order_items" explore calculating the total order count.
```

#### ⚙️ Under the Hood (MCP Tools Triggered):
1. **`validate_project`** (`{"project_id": "thelook_ecommerce"}`)
   * Result: `{"errors": [], "models_not_validated": []}` (Validation passed).
2. **`query`** (`{"model": "thelook_ecommerce", "explore": "order_items", "fields": ["order_items.count"]}`)
   * Result: `{"order_items.count": 180733}` (Query successfully executed).

---

## 📊 Summary of Artifacts & Verification URLs

| Asset | Resource / Link |
| :--- | :--- |
| **Project IDE** | [Looker IDE - thelook_ecommerce](https://c88b52e2-ebce-4d35-8f65-836444b662ba.looker.app/projects/thelook_ecommerce) |
| **Explore UI** | [Explore - order_items](https://c88b52e2-ebce-4d35-8f65-836444b662ba.looker.app/explore/thelook_ecommerce/order_items) |
| **Model File** | `thelook_ecommerce.model.lkml` |
| **Views Created** | `distribution_centers.view.lkml`, `events.view.lkml`, `inventory_items.view.lkml`, `order_items.view.lkml`, `orders.view.lkml`, `products.view.lkml`, `users.view.lkml` |
| **Validation Status** | ✅ **Passed (0 syntax or reference errors)** |
