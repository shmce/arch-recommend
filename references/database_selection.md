# Database & Storage Architecture Selection Guide

Use this guide when evaluating relational, document, key-value, column-family, search, and time-series datastores.

## Core Evaluation Dimensions

1. **Access Pattern & Data Modeling**
   * **Relational / ACID (Postgres, MySQL):** Complex joins, strict transactions, normalized domain models, unpredictable queries, schema enforcement.
   * **Document (MongoDB, DocumentDB):** Denormalized hierarchical aggregates, flexible/evolving schemas, single-document atomic updates.
   * **Key-Value / In-Memory (Redis, Valkey, DynamoDB):** High-throughput key lookups, session states, caching, rate limiters, predictable O(1) reads/writes.
   * **Wide-Column / Event-Log (Cassandra, ScyllaDB):** Extreme write throughput, time-ordered key queries, multi-region masterless clustering.
   * **Search / Analytical (Elasticsearch, OpenSearch, ClickHouse, DuckDB):** Full-text search, high-cardinality aggregations, real-time OLAP metrics.

2. **Write vs. Read Amplification & Storage Engine**
   * **B-Tree (Postgres, InnoDB):** Fast reads, moderate random writes, potential write amplification and page fragmentation under heavy concurrent append.
   * **LSM-Tree (RocksDB, Cassandra, ClickHouse):** Extremely fast sequential appends, compaction background overhead, slower random point-reads.

3. **CAP & Consistency Guarantees**
   * Strict serializable / linearizable transactions vs. read-committed vs. eventual consistency with vector clocks / CRDTs.
   * Connection pooling overhead (e.g., Postgres process-per-connection vs. threaded engines like MySQL or proxies like PgBouncer).

4. **Day 1 vs. Day 1000 Traps**
   * **Postgres Trap:** Bloat from MVCC autovacuum failure under constant updates; connection exhaustion without pooling; table partitioning retrofitting.
   * **DynamoDB Trap:** Hot partitions caused by poorly distributed partition keys; GSI throttling affecting main table writes.
   * **MongoDB Trap:** Massive unindexed working sets exhausting RAM; unbounded array growth exceeding 16MB document limit.
