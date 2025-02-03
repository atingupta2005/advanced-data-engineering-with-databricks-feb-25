# Unity Catalog Practical Guide Using UI & SQL Commands

## Table of Contents

1. [Overview](#overview)
2. [Step 1: Creating the Unity Catalog Metastore](#step-1-creating-the-unity-catalog-metastore)
3. [Step 2: Configuring Storage Credentials & External Location](#step-2-configuring-storage-credentials--external-location)
4. [Step 3: Creating a Catalog, Schema, and Tables with Sample Data](#step-3-creating-a-catalog-schema-and-tables-with-sample-data)
5. [Step 4: Loading and Querying Sample Data](#step-4-loading-and-querying-sample-data)
6. [Step 5: Setting Up Permissions](#step-5-setting-up-permissions)
7. [Step 6: Verifying and Querying via the UI](#step-6-verifying-and-querying-via-the-ui)
8. [Troubleshooting & Best Practices](#troubleshooting--best-practices)

---

## Overview

**Unity Catalog** is a centralized governance solution built into Azure Databricks. In this guide, you will:
- Create a Unity Catalog metastore.
- Set up a storage credential and external location that points to your ADLS Gen2 container.
- Create a sample catalog, schema, and tables representing a retail scenario (Customers and Orders).
- Load realistic sample data.
- Configure permissions and test queries using both the UI and SQL commands.

---

## Step 1: Creating the Unity Catalog Metastore

### Using the UI

1. **Log into your Databricks Workspace**:
   - From the Azure portal, open your Azure Databricks workspace.

2. **Access the Admin Console**:
   - In the left-hand sidebar, click on **Admin Console**.
   - Select the **Unity Catalog** tab.

3. **Create a New Metastore**:
   - Click **Create Metastore**.
   - Provide a name (for example, `RetailMetastore`).
   - Choose the appropriate region and subscription.
   - Click **Create**.  
   The metastore is now available for catalog operations.

---

## Step 2: Configuring Storage Credentials & External Location

You need to let Unity Catalog know where your external data resides.

### A. Create a Storage Credential

#### Using the UI

Some Databricks workspaces may allow you to configure storage credentials via the Unity Catalog settings:
1. Navigate to **Data** in the left sidebar.
2. Click on **Storage Credentials**.
3. Click **Create Storage Credential** and fill in the details using the service principal information.
4. Save your changes.

### B. Create an External Location

#### Using the UI

1. In the Databricks workspace, navigate to **Data** > **External Locations**.
2. Click **Create External Location**.
3. Enter the URL (as shown above) and select the storage credential you created.
4. Click **Create**.

---

## Step 3: Creating a Catalog, Schema, and Tables with Sample Data

Now, we’ll create a new catalog (logical container), a schema (namespace), and two tables—**Customers** and **Orders**—to represent realistic retail data.

### A. Create a Catalog

#### Using the UI

1. In the **Data** tab, click **Create Catalog**.
2. Enter the catalog name (e.g., `RetailCatalog`).
3. Select the metastore (`RetailMetastore`) if prompted.
4. Click **Create**.

#### Using SQL Commands

```sql
CREATE CATALOG RetailCatalog;
```

### B. Create a Schema

Switch to the catalog and create a schema named `PublicData`.

#### Using the UI

1. In **Data**, select your new catalog (`RetailCatalog`).
2. Click **Create Schema**.
3. Enter the schema name `PublicData` and click **Create**.

#### Using SQL Commands

```sql
USE CATALOG RetailCatalog;
CREATE SCHEMA PublicData;
```

### C. Create Tables with Sample Data

We will create two tables: `Customers` and `Orders`.

#### Table 1: Customers

**Definition:**
- `customer_id` (INT)
- `first_name` (STRING)
- `last_name` (STRING)
- `email` (STRING)
- `join_date` (DATE)

**SQL Command:**
```sql
USE RetailCatalog.PublicData;

CREATE TABLE Customers (
  customer_id INT,
  first_name STRING,
  last_name STRING,
  email STRING,
  join_date DATE
)
USING DELTA
LOCATION 'abfss://unity-catalog-data@<storage_account_name>.dfs.core.windows.net/retail/customers';
```
Replace `<storage_account_name>` with your storage account name.

#### Table 2: Orders

**Definition:**
- `order_id` (INT)
- `customer_id` (INT)
- `order_date` (DATE)
- `amount` DECIMAL(10,2)

**SQL Command:**
```sql
CREATE TABLE Orders (
  order_id INT,
  customer_id INT,
  order_date DATE,
  amount DECIMAL(10,2)
)
USING DELTA
LOCATION 'abfss://unity-catalog-data@<storage_account_name>.dfs.core.windows.net/retail/orders';
```

---

## Step 4: Loading and Querying Sample Data

We now load sample data into our tables. You can run these commands in a notebook cell or the SQL editor.

### A. Insert Sample Data into Customers

```sql
INSERT INTO Customers VALUES
  (1, 'Alice', 'Smith', 'alice.smith@example.com', '2023-01-15'),
  (2, 'Bob', 'Johnson', 'bob.johnson@example.com', '2023-02-10'),
  (3, 'Carol', 'Williams', 'carol.williams@example.com', '2023-03-05');
```

### B. Insert Sample Data into Orders

```sql
INSERT INTO Orders VALUES
  (101, 1, '2023-04-01', 150.75),
  (102, 2, '2023-04-03', 200.50),
  (103, 1, '2023-04-05', 99.99),
  (104, 3, '2023-04-07', 250.00);
```

### C. Query the Data

Verify that your data is loaded correctly:

```sql
SELECT * FROM Customers;
```

```sql
SELECT * FROM Orders;
```

You should see the inserted rows for both tables.

---

## Step 5: Setting Up Permissions

Now, grant sample users (or groups) appropriate access to your data assets.

### Using SQL Commands

For example, to grant read access to a group (e.g., `analyst_team@example.com`):

```sql
-- Grant usage on the catalog
GRANT USAGE ON CATALOG RetailCatalog TO `analyst_team@example.com`;

-- Grant usage on the schema
GRANT USAGE ON SCHEMA RetailCatalog.PublicData TO `analyst_team@example.com`;

-- Grant select on the tables
GRANT SELECT ON TABLE RetailCatalog.PublicData.Customers TO `analyst_team@example.com`;
GRANT SELECT ON TABLE RetailCatalog.PublicData.Orders TO `analyst_team@example.com`;
```

### Using the UI

1. Navigate to **Data** and locate your catalog (`RetailCatalog`) and then the schema (`PublicData`).
2. Click on **Permissions**.
3. Use the UI to add a user or group (e.g., `analyst_team@example.com`) and assign the appropriate roles (such as **Viewer** or **Data Reader**).
4. Save your changes.

---

## Step 6: Verifying and Querying via the UI

### A. Using the Databricks SQL Editor

1. Open the **SQL Editor** in your Azure Databricks workspace.
2. Choose the catalog `RetailCatalog` from the dropdown.
3. Run your sample queries:
   - `SELECT * FROM PublicData.Customers;`
   - `SELECT * FROM PublicData.Orders;`

### B. Using Notebooks

1. Create a new notebook.
2. Set the default catalog and schema:
   ```sql
   USE CATALOG RetailCatalog;
   USE PublicData;
   ```
3. Execute your queries:
   ```sql
   SELECT * FROM Customers;
   SELECT * FROM Orders;
   ```

The UI-based query results should match the data you inserted.

---

## Troubleshooting & Best Practices

- **Connectivity Issues:**  
  Ensure that your cluster has network access to your ADLS Gen2 account. Check firewall rules or VNet configurations if data cannot be reached.

- **Permission Denied:**  
  Double-check that the service principal and user accounts have the proper permissions on both the Databricks side (Unity Catalog) and the storage side (ADLS Gen2).

- **Data Consistency:**  
  When using external locations, data may be stored outside of Databricks. Confirm that file paths in your `LOCATION` clause are correct and that files are not accidentally modified outside Databricks.

- **Regular Auditing:**  
  Use Unity Catalog’s auditing capabilities to track changes, access, and data lineage. This is especially useful for compliance and troubleshooting.

- **UI vs. SQL:**  
  The UI is excellent for beginners and quick tasks. For automation and reproducibility, SQL commands and notebooks are preferred.
