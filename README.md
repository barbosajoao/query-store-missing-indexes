# query-store-missing-indexes

SQL Server's built-in missing index DMVs show *what* indexes are missing, but not *how much* they actually cost in production. This script solves that by cross-referencing missing index recommendations with **Query Store** execution data to calculate real-world impact.

## What makes this different

| Standard DMV approach | This script |
|---|---|
| Estimates impact from seek/scan counts only | Calculates impact from actual execution time (Query Store) |
| Includes all hours (batch jobs, off-hours noise) | Filters for business hours only (Mon-Fri, 07:30-18:30) |
| No memory/IO context | Shows min/avg/max memory and logical reads per missing index |
| No write cost awareness | Shows write activity on the table's clustered index |
| No index saturation check | Shows how many indexes the table already has |

## Key output columns

- **qs_calculated_impact** — Total execution time (ms) x estimated improvement %. The main ranking metric.
- **qs_total_executions** — Number of executions during business hours (from Query Store).
- **qs_avg_duration_ms** — Average duration of affected queries.
- **avg_memory_mb / avg_logical_reads_mb** — Resource consumption of affected queries.
- **writes_on_pk** — Write activity on the table's clustered index (helps assess index maintenance cost).
- **existing_index_count** — Total indexes on the table (avoid over-indexing).
- **create_statement** — Ready-to-use CREATE INDEX DDL.

## Requirements

- SQL Server 2019 or later
- Query Store enabled on the target database
- Execute in the context of the target database (`USE [YourDatabaseName]`)

## Usage

```sql
USE [YourDatabaseName];
GO
-- Then run the script
```

## Credits

- **Original Author:** Pinal Dave ([SQLAuthority.com](https://blog.sqlauthority.com/2011/01/03/sql-server-2008-missing-index-script-download/))
- **Extended by:** Joao Barbosa

## License

:warning: License pending original author approval.
