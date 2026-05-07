# Database Standards

These standards apply to all database work across Condado engagements. We work primarily in **SQL Server** and **PostgreSQL** (targeting Azure Database for PostgreSQL — Flexible Server). Section 1 covers shared principles. Sections 2–6 address SQL Server-specific patterns. Section 7 covers PostgreSQL differences and migration guidance.

When citing a rule in a pull request or code review, use the section code in brackets (e.g., `[DB-NAME-03]`). For PostgreSQL-specific rules, use `[PG-*]` codes. Codes are stable.

---

## 1. Shared Principles

### 1.1 Keep Logic in the Application Layer [DB-CORE-01]

Where possible, business logic lives in the application layer, not in the database. Stored procedures and database functions are appropriate for performance-critical operations and bulk data processing, not for general application logic. This keeps the database engine swappable and the codebase testable.

### 1.2 Document Deviations [DB-CORE-02]

When you denormalize for performance, use a stored procedure instead of application logic, or make any other intentional deviation from these standards, document the decision in the migration script or an ADR. Undocumented exceptions become tech debt.

### 1.3 Schema Changes Are Code [DB-CORE-03]

Every schema change goes through a migration script tracked in source control. No manual DDL changes in shared or production environments. Migrations are reviewed and approved like any other code change.

---

## 2. Naming Conventions — SQL Server [DB-NAME]

### 2.1 General Principles [DB-NAME-01]

- **PascalCase** for all database object names: tables, columns, views, stored procedures, functions, indexes, constraints
- **Singular nouns** for table names: `Recording`, not `Recordings`
- No type-encoding prefixes: no `tbl_`, `sp_`, `vw_`, `fn_`. The object type is visible in tooling — the prefix is noise. The `sp_` prefix is specifically reserved by SQL Server for system procedures and must never be used.
- No abbreviations unless universally understood (`Id`, `URL`, `SSO`)
- No spaces, special characters, or reserved words in object names

### 2.2 Table Names [DB-NAME-02]

- PascalCase, singular: `Recording`, `User`, `Organization`, `RetentionPolicy`
- Junction tables concatenate both entity names alphabetically: `OrganizationUser`, `RecordingTag`
- Audit tables append `AuditLog`: `RecordingAuditLog`
- Staging/ETL tables use a `Staging` prefix: `StagingRecordingImport`

### 2.3 Column Names [DB-NAME-03]

- PascalCase: `FirstName`, `CreatedAt`, `IsActive`
- Primary key: `Id` (just `Id` — the table name already provides context)
- Foreign keys: `<ReferencedTable>Id` — `OrganizationId`, `CreatedByUserId`
- Boolean columns: `Is`, `Has`, `Can`, or `Should` prefix — `IsActive`, `HasAccess`
- Timestamp columns: `At` suffix — `CreatedAt`, `ModifiedAt`
- Date-only columns: `On` suffix — `ExpiresOn`
- Never reuse a column name with a different meaning across tables

### 2.4 Constraint Names [DB-NAME-04]

| Constraint | Pattern | Example |
|---|---|---|
| Primary key | `PK_<Table>` | `PK_Recording` |
| Foreign key | `FK_<Table>_<Referenced>` | `FK_Recording_Organization` |
| Unique | `UQ_<Table>_<Column(s)>` | `UQ_User_Email` |
| Check | `CK_<Table>_<Column>` | `CK_Recording_Status` |
| Default | `DF_<Table>_<Column>` | `DF_Recording_CreatedAt` |

### 2.5 Index Names [DB-NAME-05]

| Type | Pattern | Example |
|---|---|---|
| Clustered | `CX_<Table>` | `CX_Recording` |
| Nonclustered | `IX_<Table>_<Column(s)>` | `IX_Recording_OrganizationId` |
| Unique nonclustered | `UX_<Table>_<Column(s)>` | `UX_User_Email` |
| Filtered | `IX_<Table>_<Column>_Where<Condition>` | `IX_Recording_Status_WhereActive` |

---

## 3. Schema Design [DB-SCHEMA]

### 3.1 Normalization [DB-SCHEMA-01]

Design to Third Normal Form (3NF) as the baseline — every column depends on the key, the whole key, and nothing but the key. Denormalize deliberately and never accidentally. When you denormalize for read performance, document which queries benefit, what the write cost is, and how staleness is managed.

### 3.2 Primary Keys [DB-SCHEMA-02]

Use `NEWSEQUENTIALID()` (SQL Server) or a `uuid_generate_v4()` equivalent (PostgreSQL) for new tables to avoid page fragmentation while preserving uniqueness across systems. Integer identity columns are acceptable when the table is internal and never synced or referenced across systems.

### 3.3 Soft Deletes [DB-SCHEMA-03]

Prefer soft deletes (`IsDeleted BIT`, `DeletedAt DATETIME2`) over hard deletes for any entity that might need audit history or recovery. Hard deletes are only appropriate for transient or truly expendable data.

### 3.4 Audit Columns [DB-SCHEMA-04]

Every user-facing entity table includes: `CreatedAt`, `CreatedByUserId`, `ModifiedAt`, `ModifiedByUserId`. These are populated at the application layer, not via database triggers.

---

## 4. Query Standards [DB-QUERY]

### 4.1 Explicit Column Lists [DB-QUERY-01]

Never use `SELECT *` in production code. List columns explicitly. This prevents unexpected data exposure and makes schema changes detectable at query time rather than at runtime.

### 4.2 Parameterize Everything [DB-QUERY-02]

Every query that accepts external input uses parameterized queries or an ORM with parameterization (EF Core). Never concatenate user input into SQL. This is non-negotiable.

### 4.3 Avoid Implicit Conversions [DB-QUERY-03]

Use the correct data type in comparisons. Implicit type conversions defeat index seeks and cause full scans. If a comparison requires a cast, fix the schema or the query — not the where clause.

### 4.4 Pagination [DB-QUERY-04]

Use `OFFSET`/`FETCH` (SQL Server) or `LIMIT`/`OFFSET` (PostgreSQL) for pagination. Never use `SELECT TOP n` for paging. Large offset values are slow — use keyset pagination (cursor-based) for high-volume tables.

---

## 5. Transactions [DB-TXN]

### 5.1 Keep Transactions Short [DB-TXN-01]

Transactions should be as short as possible. Long-running transactions hold locks and block other readers and writers. Do all non-database work before opening a transaction, not inside one.

### 5.2 Explicit Rollback [DB-TXN-02]

Every explicit transaction has a `CATCH`/`ROLLBACK` block. Never let a failed transaction silently succeed a partial commit.

### 5.3 Isolation Level [DB-TXN-03]

Use `READ COMMITTED` as the default. Use `SNAPSHOT` isolation for read-heavy operations that need consistent views without blocking writers. Document any deviation from the default isolation level with the reason.

---

## 6. Change Management [DB-CHG]

### 6.1 Migration Scripts [DB-CHG-01]

Every schema change is a migration script — forward only, no destructive rollbacks. Migrations are:

- Named with a timestamp prefix: `20260415_AddRetentionPolicyTable.sql`
- Idempotent where possible (check before alter)
- Reviewed and approved in a PR before running in any shared environment
- Run in order in CI before the application deploys

### 6.2 Breaking vs. Non-Breaking Changes [DB-CHG-02]

Prefer non-breaking changes: add columns before removing old ones, add indexes before dropping old ones. When a breaking change is unavoidable, deploy in two phases: migrate schema, then release the application code that removes the old path.

### 6.3 No Manual Changes in Shared Environments [DB-CHG-03]

Manual DDL changes to dev, staging, or production environments are not permitted. Every change goes through source control and the deployment pipeline. There are no exceptions.

---

## 7. PostgreSQL Differences [PG]

These sections apply specifically to PostgreSQL engagements or when migrating from SQL Server.

### 7.1 Naming: snake_case [PG-NAME-01]

PostgreSQL folds unquoted identifiers to lowercase. Use `snake_case` for all object names — tables, columns, functions, indexes, constraints. This is the PostgreSQL community convention and avoids the need for double-quoting.

| SQL Server | PostgreSQL |
|---|---|
| `Recording` | `recording` |
| `OrganizationId` | `organization_id` |
| `IsActive` | `is_active` |
| `CreatedAt` | `created_at` |

Note: `user` is a reserved word in PostgreSQL. Use `user_account` instead.

### 7.2 Constraint and Index Naming [PG-NAME-02]

PostgreSQL's identifier limit is 63 characters (vs. 128 in SQL Server). Follow the same patterns as SQL Server but in snake_case:

- Primary key: `pk_<table>` → `pk_recording`
- Foreign key: `fk_<table>_<referenced>` → `fk_recording_organization`
- Unique: `uq_<table>_<column>` → `uq_user_account_email`
- Index: `ix_<table>_<column>` → `ix_recording_org_id`

### 7.3 Schema Naming [PG-NAME-03]

Use `core` (not `dbo`) for core entities. Tighten the default `public` schema grants — don't rely on permissive defaults. Set `search_path` deliberately in application connection strings.

### 7.4 No Stored Procedures for Logic [PG-FUNC-01]

Prior to PostgreSQL 11, there were no stored procedures — only functions. Use `FUNCTION` for operations that return data. Use `PROCEDURE` (PostgreSQL 11+) only for operations that need to manage their own transactions. Business logic belongs in the application layer.

### 7.5 JSONB [PG-JSON-01]

Use `JSONB` (not `JSON`) for semi-structured data — it's stored in a binary format that supports indexing. Index JSONB columns with a GIN index on columns queried frequently. Don't use JSONB as a substitute for proper schema design — if you're querying the same JSONB key in every query, it should be a column.

### 7.6 Connection Pooling [PG-POOL-01]

PostgreSQL's process-per-connection model means unbounded connections are expensive. Use PgBouncer or the connection pooler built into Azure Database for PostgreSQL. Configure pool size based on expected load and available server resources, not by defaulting to "as many as possible."

### 7.7 EF Core Naming Convention [PG-EF-01]

When using EF Core with PostgreSQL, apply `UseSnakeCaseNamingConvention()` from the `EFCore.NamingConventions` package. This maps PascalCase C# properties to snake_case PostgreSQL columns automatically.
