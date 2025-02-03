### **Unity Catalog Permissions**

### **1. Metastore Permissions**

The **metastore** in Unity Catalog is a container for **catalogs** and **schemas**. Permissions at the metastore level are focused on the **creation** and **management** of catalogs and schemas. There is **no "USE CATALOG" permission** at the metastore level.

#### **Privileged Commands for Metastore:**

| Permission         | Description                                                           | Command Example                                                    |
|--------------------|-----------------------------------------------------------------------|--------------------------------------------------------------------|
| **CREATE CATALOG**  | Allows the user to create a new catalog within the metastore.          | `GRANT CREATE CATALOG ON METASTORE TO 'username';`                 |
| **MANAGE GRANTS**   | Allows the user to manage permissions at the metastore level.         | `GRANT MANAGE GRANTS ON METASTORE TO 'username';`                  |

**Important Notes:**
- **USE CATALOG** cannot be granted at the metastore level, as it applies only to catalogs and schemas.
- At the metastore level, the **CREATE CATALOG** and **MANAGE GRANTS** permissions are the main types available.

---

### **2. Catalog Permissions**

A **catalog** is a logical container in Unity Catalog that houses schemas. Permissions at this level govern access to the catalog and its schemas.

#### **Privileged Commands for Catalogs:**

| Permission         | Description                                                       | Command Example                                                    |
|--------------------|-------------------------------------------------------------------|--------------------------------------------------------------------|
| **CREATE SCHEMA**   | Allows the user to create a new schema in a catalog.              | `GRANT CREATE SCHEMA ON CATALOG <catalog_name> TO 'username';`     |
| **USE CATALOG**     | Allows the user to access and use the catalog.                    | `GRANT USE CATALOG ON CATALOG <catalog_name> TO 'username';`       |
| **MANAGE GRANTS**   | Allows the user to manage permissions at the catalog level.       | `GRANT MANAGE GRANTS ON CATALOG <catalog_name> TO 'username';`     |

**Important Notes:**
- **USE CATALOG** is valid at the catalog level to allow users to access metadata and interact with the catalog's schemas.
- Permissions at the catalog level do not include table-level operations; those are managed at the schema or table level.

---

### **3. Schema Permissions**

A **schema** resides within a catalog and contains tables, views, and other data objects. Permissions at this level govern access to the schema’s contents.

#### **Privileged Commands for Schemas:**

| Permission         | Description                                                           | Command Example                                                     |
|--------------------|-----------------------------------------------------------------------|---------------------------------------------------------------------|
| **CREATE TABLE**    | Allows the user to create new tables in a schema.                     | `GRANT CREATE TABLE ON SCHEMA <catalog_name>.<schema_name> TO 'username';`  |
| **USE SCHEMA**      | Allows the user to access and query tables within a schema.          | `GRANT USE SCHEMA ON SCHEMA <catalog_name>.<schema_name> TO 'username';`     |
| **CREATE VIEW**     | Allows the user to create new views in the schema.                    | `GRANT CREATE VIEW ON SCHEMA <catalog_name>.<schema_name> TO 'username';`    |
| **MANAGE GRANTS**   | Allows the user to manage permissions at the schema level.           | `GRANT MANAGE GRANTS ON SCHEMA <catalog_name>.<schema_name> TO 'username';`  |

**Important Notes:**
- **USE SCHEMA** is granted at the schema level, enabling users to access tables and views within a schema.
- **CREATE TABLE** and **CREATE VIEW** allow users to create data objects within a schema.

---

### **4. Table Permissions**

A **table** stores data within a schema. Permissions on tables control access to data and allow users to perform operations like reading, inserting, updating, and deleting data.

#### **Privileged Commands for Tables:**

| Permission         | Description                                                           | Command Example                                                     |
|--------------------|-----------------------------------------------------------------------|---------------------------------------------------------------------|
| **SELECT**          | Allows the user to read data from the table.                          | `GRANT SELECT ON TABLE <catalog_name>.<schema_name>.<table_name> TO 'username';` |
| **INSERT**          | Allows the user to insert data into the table.                        | `GRANT INSERT ON TABLE <catalog_name>.<schema_name>.<table_name> TO 'username';` |
| **UPDATE**          | Allows the user to update data in the table.                          | `GRANT UPDATE ON TABLE <catalog_name>.<schema_name>.<table_name> TO 'username';` |
| **DELETE**          | Allows the user to delete data from the table.                        | `GRANT DELETE ON TABLE <catalog_name>.<schema_name>.<table_name> TO 'username';` |
| **TRUNCATE**        | Allows the user to truncate the table (removes all rows).            | `GRANT TRUNCATE ON TABLE <catalog_name>.<schema_name>.<table_name> TO 'username';` |
| **ALTER**           | Allows the user to alter the table’s structure (e.g., add/remove columns). | `GRANT ALTER ON TABLE <catalog_name>.<schema_name>.<table_name> TO 'username';` |
| **DROP**            | Allows the user to drop the table.                                    | `GRANT DROP ON TABLE <catalog_name>.<schema_name>.<table_name> TO 'username';` |

**Important Notes:**
- **SELECT**, **INSERT**, **UPDATE**, **DELETE**, **TRUNCATE**, **ALTER**, and **DROP** are common permissions for table management.
- These permissions allow full control over table data and structure.

---

### **5. View Permissions**

A **view** is a virtual table defined by a query. Users can interact with views in a similar way to tables, but views are typically read-only unless designed otherwise.

#### **Privileged Commands for Views:**

| Permission         | Description                                                           | Command Example                                                     |
|--------------------|-----------------------------------------------------------------------|---------------------------------------------------------------------|
| **SELECT**          | Allows the user to read data from the view.                           | `GRANT SELECT ON VIEW <catalog_name>.<schema_name>.<view_name> TO 'username';` |
| **CREATE VIEW**     | Allows the user to create new views in the schema.                    | `GRANT CREATE VIEW ON SCHEMA <catalog_name>.<schema_name> TO 'username';`    |

**Important Notes:**
- **SELECT** allows users to query a view.
- **CREATE VIEW** allows users to create views within a schema.

---

### **6. External Location Permissions**

An **external location** is a reference to a data storage resource (e.g., AWS S3, Azure Data Lake) used for accessing external data.

#### **Privileged Commands for External Locations:**

| Permission         | Description                                                           | Command Example                                                     |
|--------------------|-----------------------------------------------------------------------|---------------------------------------------------------------------|
| **USE EXTERNAL LOCATION** | Allows the user to access and use an external location for reading/writing data. | `GRANT USE EXTERNAL LOCATION ON LOCATION <location_name> TO 'username';` |
| **MANAGE EXTERNAL LOCATION** | Allows the user to manage external locations, including creation and configuration. | `GRANT MANAGE EXTERNAL LOCATION ON LOCATION <location_name> TO 'username';` |

**Important Notes:**
- **USE EXTERNAL LOCATION** grants access to external data storage.
- **MANAGE EXTERNAL LOCATION** allows full administrative control over the external location.

---

### **7. Credential Permissions**

**Credentials** are used to authenticate access to external resources (e.g., cloud storage, databases).

#### **Privileged Commands for Credentials:**

| Permission         | Description                                                           | Command Example                                                     |
|--------------------|-----------------------------------------------------------------------|---------------------------------------------------------------------|
| **USE CREDENTIAL**  | Allows the user to use a credential for accessing external resources. | `GRANT USE CREDENTIAL ON CREDENTIAL <credential_name> TO 'username';` |
| **MANAGE CREDENTIAL** | Allows the user to create, modify, and manage credentials.          | `GRANT MANAGE CREDENTIAL ON CREDENTIAL <credential_name> TO 'username';` |

**Important Notes:**
- **USE CREDENTIAL** is necessary for accessing external systems that require authentication.
- **MANAGE CREDENTIAL** allows users to configure credentials.

---

### **8. Volume Permissions**

A **volume** is a persistent storage system in Databricks for managing notebooks, libraries, and other assets.

#### **Privileged Commands for Volumes:**

| Permission         | Description                                                           | Command Example                                                     |
|--------------------|-----------------------------------------------------------------------|---------------------------------------------------------------------|
| **USE VOLUME**      | Allows the user to access and use the volume for storing assets.      | `GRANT USE VOLUME ON VOLUME <volume_name> TO 'username';`           |
| **MANAGE VOLUME**   | Allows the user to create, delete, and manage volumes.                | `GRANT MANAGE VOLUME ON VOLUME <volume_name> TO 'username';`        |

**Important Notes:**
- **USE VOLUME** allows users to interact with data stored in volumes.
- **MANAGE VOLUME** gives administrative control over volumes.

---

### **Final Example Scenario:**

Let's assume you want to grant the **Data Analyst Group** access to:

1. **RetailCatalog** (catalog)
   - **USE CATALOG** permission.
2. **RetailCatalog.RetailSchema** (schema)
   - **USE SCHEMA** permission.
3. **RetailCatalog.RetailSchema.Customers** (table)
   - **SELECT** permission.
4. **External Storage** (external location)
   - **USE EXTERNAL LOCATION** permission.

Here are the commands:

```sql
-- Grant USE CATALOG on RetailCatalog
GRANT USE CATALOG ON CATALOG RetailCatalog TO 'data_analyst_group';

-- Grant USE SCHEMA on RetailCatalog.RetailSchema
GRANT USE SCHEMA ON SCHEMA RetailCatalog.RetailSchema TO 'data_analyst_group';

-- Grant SELECT on RetailCatalog.RetailSchema.Customers table
GRANT SELECT ON TABLE RetailCatalog.RetailSchema.Customers TO 'data_analyst_group';

-- Grant USE EXTERNAL LOCATION on external_storage
GRANT USE EXTERNAL LOCATION ON LOCATION external_storage TO 'data_analyst_group';
```

---
