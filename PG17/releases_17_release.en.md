September 26, 2024 - The [PostgreSQL Global Development Group](https://www.postgresql.org)
today announced the release of [PostgreSQL 17](https://www.postgresql.org/docs/17/release-17.html),
the latest version of the world's most advanced open source database.

PostgreSQL 17 builds on decades of open source development, improving its
performance and scalability while adapting to emergent data access and storage
patterns. This release of [PostgreSQL](https://www.postgresql.org) adds
significant overall performance gains, including an overhauled memory management
implementation for vacuum, optimizations to storage access and improvements for
high concurrency workloads, speedups in bulk loading and exports, and query
execution improvements for indexes. PostgreSQL 17 has features that benefit
brand new workloads and critical systems alike, such as additions to the
developer experience with the SQL/JSON `JSON_TABLE` command, and enhancements to
logical replication that simplify management of high availability workloads and
major version upgrades.

"PostgreSQL 17 highlights how the global open source community, which drives the
development of PostgreSQL, builds enhancements that help users at all stages of
their database journey," said Jonathan Katz, a member of the PostgreSQL core
team. "Whether it's improvements for operating databases at scale or
new features that build on a delightful developer experience, PostgreSQL 17
will enhance your data management experience." 

PostgreSQL, an innovative data management system known for its reliability,
robustness, and extensibility, benefits from over 25 years of open source
development from a global developer community and has become the preferred open
source relational database for organizations of all sizes.

### System-wide performance gains

The PostgreSQL [vacuum](https://www.postgresql.org/docs/17/routine-vacuuming.html)
process is critical for healthy operations, requiring server instance resources
to operate. PostgreSQL 17 introduces a new internal memory structure for vacuum
that consumes up to 20x less memory. This improves vacuum speed and
also reduces the use of shared resources, making more available for your
workload.

PostgreSQL 17 continues to improve performance of its I/O layer. High
concurrency workloads may see up to 2x better write throughput due to
improvements with [write-ahead log](https://www.postgresql.org/docs/17/wal-intro.html)
([WAL](https://www.postgresql.org/docs/17/wal-intro.html)) processing.
Additionally, the new streaming I/O interface speeds up sequential scans
(reading all the data from a table) and how quickly
[`ANALYZE`](https://www.postgresql.org/docs/17/sql-analyze.html) can update
planner statistics.

PostgreSQL 17 also extends its performance gains to query execution.
PostgreSQL 17 improves the performance of queries with `IN` clauses that use
[B-tree](https://www.postgresql.org/docs/17/indexes-types.html#INDEXES-TYPES-BTREE)
indexes, the default index method in PostgreSQL. Additionally,
[BRIN](https://www.postgresql.org/docs/17/brin.html) indexes now support
parallel builds. PostgreSQL 17 includes several improvements for query planning,
including optimizations for `NOT NULL` constraints, and improvements in
processing [common table expressions](https://www.postgresql.org/docs/17/queries-with.html)
([`WITH` queries](https://www.postgresql.org/docs/17/queries-with.html)). This
release adds more SIMD (Single Instruction/Multiple Data) support for
accelerating computations, including using AVX-512 for the
[`bit_count`](https://www.postgresql.org/docs/17/functions-bitstring.html)
function.

### Further expansion of a robust developer experience

PostgreSQL was the [first relational database to add JSON support (2012)](https://www.postgresql.org/about/news/postgresql-92-released-1415/),
and PostgreSQL 17 adds to its implementation of the SQL/JSON standard.
[`JSON_TABLE`](https://www.postgresql.org/docs/17/functions-json.html#FUNCTIONS-SQLJSON-TABLE)
is now available in PostgreSQL 17, letting developers convert JSON data into a
standard PostgreSQL table. PostgreSQL 17 now supports [SQL/JSON constructors](https://www.postgresql.org/docs/17/functions-json.html#FUNCTIONS-JSON-CREATION-TABLE)
(`JSON`, `JSON_SCALAR`, `JSON_SERIALIZE`) and
[query functions](https://www.postgresql.org/docs/17/functions-json.html#SQLJSON-QUERY-FUNCTIONS)
(`JSON_EXISTS`, `JSON_QUERY`, `JSON_VALUE`), giving developers other ways of
interfacing with their JSON data. This release adds more
[`jsonpath` expressions](https://www.postgresql.org/docs/17/functions-json.html#FUNCTIONS-SQLJSON-PATH-OPERATORS),
with an emphasis of converting JSON data to a native PostgreSQL data type,
including numeric, boolean, string, and date/time types.

PostgreSQL 17 adds more features to [`MERGE`](https://www.postgresql.org/docs/17/sql-merge.html),
which is used for conditional updates, including a `RETURNING` clause and the
ability to update [views](https://www.postgresql.org/docs/17/sql-createview.html).
Additionally, PostgreSQL 17 has new capabilities for bulk loading and data
exporting, including up to a 2x performance improvement when exporting large rows
using the [`COPY`](https://www.postgresql.org/docs/17/sql-copy.html) command.
`COPY` performance also has improvements when the source and destination
encodings match, and includes a new option, `ON_ERROR`, that allows an import to
continue even if there is an insert error.

This release expands on functionality both for managing data in partitions and
data distributed across remote PostgreSQL instances. PostgreSQL 17 supports
using identity columns and exclusion constraints on
[partitioned tables](https://www.postgresql.org/docs/17/ddl-partitioning.html).
The [PostgreSQL foreign data wrapper](https://www.postgresql.org/docs/17/postgres-fdw.html)
([`postgres_fdw`](https://www.postgresql.org/docs/17/postgres-fdw.html)), used
to execute queries on remote PostgreSQL instances, can now push `EXISTS` and
`IN` subqueries to the remote server for more efficient processing.

PostgreSQL 17 also includes a built-in, platform independent, immutable
collation provider that's guaranteed to be immutable and provides similar
sorting semantics to the `C` collation except with `UTF-8` encoding rather than
`SQL_ASCII`. Using this new collation provider guarantees that your text-based
queries will return the same sorted results regardless of where you run
PostgreSQL.

### Logical replication enhancements for high availability and major version upgrades

[Logical replication](https://www.postgresql.org/docs/17/logical-replication.html)
is used to stream data in real-time across many use cases. However, prior to
this release, users who wanted to perform a major version upgrade would have to
drop [logical replication slots](https://www.postgresql.org/docs/17/logical-replication-subscription.html#LOGICAL-REPLICATION-SUBSCRIPTION-SLOT), which requires resynchronizing data
to subscribers after an upgrade. Starting with upgrades from PostgreSQL 17,
users don't have to drop logical replication slots, simplifying the upgrade
process when using logical replication.

PostgreSQL 17 now includes failover control for logical replication, making it
more resilient when deployed in high availability environments. Additionally,
PostgreSQL 17 introduces the
[`pg_createsubscriber`](https://www.postgresql.org/docs/17/app-pgcreatesubscriber.html)
command-line tool for converting a physical replica into a new logical replica.

### More options for managing security and operations

PostgreSQL 17 further extends how users can manage the overall lifecycle of
their database systems. PostgreSQL has a new TLS option, `sslnegotiation`, that
lets users perform a direct TLS handshakes when using
[ALPN](https://en.wikipedia.org/wiki/Application-Layer_Protocol_Negotiation)
(registered as `postgresql` in the ALPN directory). PostgreSQL 17 also adds the
`pg_maintain` [predefined role](https://www.postgresql.org/docs/17/predefined-roles.html),
which gives users permission to perform maintenance operations.

[`pg_basebackup`](https://www.postgresql.org/docs/17/app-pgbasebackup.html), the
backup utility included in PostgreSQL, now supports incremental backups and adds
the [`pg_combinebackup`](https://www.postgresql.org/docs/17/app-pgcombinebackup.html) 
utility to reconstruct a full backup. Additionally,
[`pg_dump`](https://www.postgresql.org/docs/17/app-pgdump.html) includes a new
option called `--filter` that lets you select what objects to include when
generating a dump file.

PostgreSQL 17 also includes enhancements to monitoring and analysis features.
[`EXPLAIN`](https://www.postgresql.org/docs/17/sql-explain.html) now shows the
time spent for local I/O block reads and writes, and includes two new options:
`SERIALIZE` and `MEMORY`, useful for seeing the time spent in data conversion
for network transmission, and how much memory was used. PostgreSQL 17 now
reports the [progress of vacuuming indexes](https://www.postgresql.org/docs/17/progress-reporting.html#VACUUM-PROGRESS-REPORTING),
and adds the [`pg_wait_events`](https://www.postgresql.org/docs/17/view-pg-wait-events.html)
system view that, when combined with [`pg_stat_activity`](https://www.postgresql.org/docs/17/monitoring-stats.html#MONITORING-PG-STAT-ACTIVITY-VIEW),
gives more insight into why an active session is waiting.

### Additional Features

Many other new features and improvements have been added to PostgreSQL 17 that
may also be helpful for your use cases. Please see the
[release notes](https://www.postgresql.org/docs/17/release-17.html) for a
complete list of new and changed features.

### About PostgreSQL

[PostgreSQL](https://www.postgresql.org) is the world's most advanced open
source database, with a global community of thousands of users, contributors,
companies and organizations. Built on over 35 years of engineering, starting at
the University of California, Berkeley, PostgreSQL has continued with an
unmatched pace of development. PostgreSQL's mature feature set not only matches
top proprietary database systems, but exceeds them in advanced database
features, extensibility, security, and stability.

### Links

* [Download](https://www.postgresql.org/download/)
* [Release Notes](https://www.postgresql.org/docs/17/release-17.html)
* [Press Kit](https://www.postgresql.org/about/press/)
* [Security Page](https://www.postgresql.org/support/security/)
* [Versioning Policy](https://www.postgresql.org/support/versioning/)
* [Follow @postgresql](https://twitter.com/postgresql)
* [Donate](https://www.postgresql.org/about/donate/)

## More About the Features

For explanations of the above features and others, please see the following
resources:

* [Release Notes](https://www.postgresql.org/docs/17/release-17.html)
* [Feature Matrix](https://www.postgresql.org/about/featurematrix/)

## Where to Download

There are several ways you can download PostgreSQL 17, including:

* The [Official Downloads](https://www.postgresql.org/download/) page, with contains installers and tools for [Windows](https://www.postgresql.org/download/windows/), [Linux](https://www.postgresql.org/download/linux/), [macOS](https://www.postgresql.org/download/macosx/), and more.
* [Source Code](https://www.postgresql.org/ftp/source/v17.0)

Other tools and extensions are available on the
[PostgreSQL Extension Network](http://pgxn.org/).

## Documentation

PostgreSQL 17 comes with HTML documentation as well as man pages, and you can also browse the documentation online in both [HTML](https://www.postgresql.org/docs/17/) and [PDF](https://www.postgresql.org/files/documentation/pdf/17/postgresql-17-US.pdf) formats.

## Licence

PostgreSQL uses the [PostgreSQL License](https://www.postgresql.org/about/licence/),
a BSD-like "permissive" license. This
[OSI-certified license](http://www.opensource.org/licenses/postgresql/) is
widely appreciated as flexible and business-friendly, since it does not restrict
the use of PostgreSQL with commercial and proprietary applications. Together
with multi-company support and public ownership of the code, our license makes
PostgreSQL very popular with vendors wanting to embed a database in their own
products without fear of fees, vendor lock-in, or changes in licensing terms.

## Contacts

Website

* [https://www.postgresql.org/](https://www.postgresql.org/)

Email

* [press@postgresql.org](mailto:press@postgresql.org)

## Images and Logos

Postgres, PostgreSQL, and the Elephant Logo (Slonik) are all registered
trademarks of the [PostgreSQL Community Association](https://www.postgres.ca).
If you wish to use these marks, you must comply with the [trademark policy](https://www.postgresql.org/about/policies/trademarks/).

## Corporate Support and Donations

PostgreSQL enjoys the support of numerous companies, who sponsor developers,
provide hosting resources, and give us financial support. See our
[sponsors](https://www.postgresql.org/about/sponsors/) page for some of these
project supporters.

There is also a large community of
[companies offering PostgreSQL Support](https://www.postgresql.org/support/professional_support/),
from individual consultants to multinational companies.

If you wish to make a financial contribution to the PostgreSQL Global
Development Group or one of the recognized community non-profit organizations,
please visit our [donations](https://www.postgresql.org/about/donate/) page.
