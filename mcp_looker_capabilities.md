# Looker MCP Capabilities

This document provides a comprehensive reference of all available Looker Model Context Protocol (MCP) tools and integration capabilities.

---

## 1. 🏗️ LookML Development & Project Management
* **Project Exploration & Structure**: Inspect LookML projects, list directories and files (`get_projects`, `get_project_files`, `get_project_directories`, `get_project_file`).
* **Code Management**: Create, edit, and delete LookML files and directories (`create_project_file`, `update_project_file`, `delete_project_file`, `create_project_directory`, `delete_project_directory`).
* **View Generation**: Automatically generate LookML views directly from database tables (`create_view_from_table`).
* **Validation & Dev Mode**: Toggle development mode (`dev_mode`) and run LookML validation to catch syntax/modeling errors (`validate_project`).

---

## 2. 🔍 Metadata & Semantic Model Inspection
* **Models & Explores**: Discover available models, explores, dimensions, measures, filters, and parameters (`get_models`, `get_explores`, `get_dimensions`, `get_measures`, `get_filters`, `get_parameters`).
* **Field Value Suggestions**: Retrieve possible filter/dimension values for specific fields (`get_field_value_suggestions`).

---

## 3. 📊 Dashboards, Looks & Embeds
* **Dashboards**: List, run, and programmatically build dashboards or add tiles and filters (`get_dashboards`, `run_dashboard`, `make_dashboard`, `add_dashboard_element`, `add_dashboard_filter`).
* **Looks**: Fetch, create, and execute Looks (`get_looks`, `make_look`, `run_look`).
* **Embedding**: Generate signed Looker embed URLs (`generate_embed_url`).

---

## 4. ⚡ Querying & Data Execution
* **Explore Queries**: Run Looker queries defined by model, explore, fields, filters, and limits (`query`).
* **SQL Queries**: Run SQL queries directly against Looker-connected databases (`query_sql`).
* **Query URLs**: Execute or generate queries via Looker query URLs (`query_url`).

---

## 5. 🗄️ Database Connection & Schema Introspection
* Discover configured connections, databases, schemas, tables, and column metadata (`get_connections`, `get_connection_databases`, `get_connection_schemas`, `get_connection_tables`, `get_connection_table_columns`).

---

## 6. 🧪 Testing & Instance Health
* **LookML Tests**: Discover and execute LookML data tests and assertions (`get_lookml_tests`, `run_lookml_tests`).
* **Health & Maintenance**: Run health checks, diagnose instance health, and identify unused/stale content (`health_analyze`, `health_pulse`, `health_vacuum`).

---

## 💡 Specialized LookML Skills & Best Practices
* **LookML Diagnostics & Debugging**: Diagnosing and resolving LookML validation errors.
* **Liquid Templating & Dynamic Dimensions/Measures**: Dynamic dimension/measure selection and conditional LookML formatting.
* **Persistent Derived Tables (PDTs)**: Best practices for performance, caching policies, and table persistence.
* **Period-over-Period (PoP) Analysis**: Implement robust YoY, MoM, and QoQ analytics in LookML.
