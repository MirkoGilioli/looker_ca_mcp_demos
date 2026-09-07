# Looker Conversational Analytics & MCP Demos Repository

Welcome to the **Looker Conversational Analytics & Model Context Protocol (MCP)** master repository. This repository provides a complete suite of instructor classroom demos, MCP capability guides, and an interactive escape-room-style student hackathon built on the **Looker Model Context Protocol (MCP)** server and BigQuery.

---

## 📚 Table of Contents

1. [Looker MCP Capabilities Reference](#-looker-mcp-capabilities-reference)
2. [Classroom MCP Demos](#-classroom-mcp-demos)
3. [TheLook Escape Room Hackathon](#-thelook-escape-room-hackathon)
4. [LookML Project & Semantic Architecture](#-lookml-project--semantic-architecture)
5. [Prerequisites & MCP Authentication](#-prerequisites--mcp-authentication)

---

## 🛠️ Looker MCP Capabilities Reference

Full overview of available Looker MCP endpoints, schema tools, and agent workflows:
* 📄 [**`mcp_looker_capabilities.md`**](./mcp_looker_capabilities.md) — Comprehensive reference covering:
  * **Section 1: LookML Development & Project Management** (`dev_mode`, `create_view_from_table`, `update_project_file`, `validate_project`).
  * **Section 2: Metadata & Semantic Model Inspection** (`get_models`, `get_explores`, `get_dimensions`, `get_measures`, `get_field_value_suggestions`).
  * **Section 3: Dashboards, Looks & Embeds** (`make_dashboard`, `add_dashboard_filter`, `add_dashboard_element`, `make_look`, `run_dashboard`).
  * **Section 4: Querying & Data Execution** (`query`, `query_sql`, `query_url`).
  * **Section 5: Connection & Schema Introspection** (`get_connections`, `get_connection_tables`, `get_connection_table_columns`).
  * **Section 6: Testing & Instance Health** (`run_lookml_tests`, `health_analyze`, `health_vacuum`).

---

## 🚀 Classroom MCP Demos

Standardized, step-by-step presenter and instructor demo guides:

* 🏗️ [**Demo 1: LookML Project Setup & Automated Modeling**](./demos/demo1_project_setup_mcp.md)
  * Discover database connections in BigQuery.
  * Auto-generate LookML view files directly from database tables.
  * Compose relational explores with primary-foreign key joins.
  * Validate LookML code and run test queries.

* 🔍 [**Demo 2: Metadata & Semantic Model Inspection**](./demos/demo2_metadata_semantic_inspection_mcp.md)
  * Discover models across the Looker instance (`get_models`).
  * Map available subject-area Explores (`get_explores`).
  * Introspect dimensions, timeframes, and data types across joined views (`get_dimensions`).
  * Discover aggregations, measures, and drill sets (`get_measures`).
  * Retrieve distinct field value suggestions for filter construction (`get_field_value_suggestions`).

* 📊 [**Demo 3: Dashboards, Looks & Business Diagnostics**](./demos/demo3_dashboards_looks_problem_diagnostic_mcp.md)
  * Enrich LookML with financial measures (gross profit margin, return rates, cancellation rates, delivery cycle times).
  * Programmatically generate an Executive Looker Dashboard (`make_dashboard`).
  * Wire global date and product category filters (`add_dashboard_filter`).
  * Populate diagnostic analytics tiles bound to filters (`add_dashboard_element`).
  * Discover top return geographic destinations and save a standalone Look (`make_look`).

---

## 🔐 TheLook Escape Room Hackathon

An interactive, story-driven conversational analytics hackathon designed for students and practitioners:

* 🧭 [**Hackathon Overview & Rules (`hackhathon/README.md`)**](./hackhathon/README.md)
* 📜 [**Participant Challenges (`hackhathon/escape_room_challenges.md`)**](./hackhathon/escape_room_challenges.md)
  * **🚪 Room 1**: The Vault of Vanishing Revenue *(25% Revenue Leakage)*
  * **🚪 Room 2**: The Hall of Cursed Inventory *(High-Return Categories like Jumpsuits & Rompers, Clothing Sets)*
  * **🚪 Room 3**: The Bermuda Triangle of Lost Deliveries *(Distribution Center Lead Times & Top Return Destinations)*
  * **🚪 Room 4**: The Phantom Traffic Trap *(Marketing Channel Cancellation Rates)*
  * **🚪 Room 5**: The Final Executive Escape Plan *(Assembling the Live Diagnostic Dashboard)*
* 🗝️ [**Instructor Solution Key (`hackhathon/instructor_solution_key.md`)**](./hackhathon/instructor_solution_key.md) — Master answers, benchmarks, and 100-point grading rubric.
* 📝 [**Team Submission Template (`hackhathon/submission_sheet.md`)**](./hackhathon/submission_sheet.md) — Standardized student answer sheet.

---

## 🏗️ LookML Project & Semantic Architecture

* **Project**: `thelook_ecommerce`
* **Model File**: `thelook_ecommerce.model.lkml`
* **Explores**:
  * `order_items` — joined with `orders`, `users`, `inventory_items`, `products`, and `distribution_centers`.
  * `events` — joined with `users`.
* **Database Connection**: `default_bigquery_connection` (Dataset: `mydataset`)

---

## ⚡ Prerequisites & MCP Authentication

1. **Configure MCP Server** (`~/.gemini/config/mcp_config.json`):
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
2. **Authenticate Session**:
   ```bash
   /mcp auth looker
   ```
