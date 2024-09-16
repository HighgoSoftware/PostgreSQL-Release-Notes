PostgreSQL全球开发组今天宣布[PostgreSQL 13](https://www.postgresql.org/docs/13/release-13.html)正式发布，
作为世界上[最先进的开源数据库](https://www.postgresql.org/)，PostgresSQL 13是目前的最新版本。

PostgreSQL 13 在索引和查找方面进行了重大改进，有利于大型数据库系统，同时包括索引的空间节省和性能提高，使用聚合或分区的查询时的更快响应，使用增强的统计信息时更优化的查询计划，以及很多其他改进。

PostgreSQL 13除了具有强烈要求的功能（如[并行清理](https://www.postgresql.org/docs/13/sql-vacuum.html)和[增量排序](https://www.postgresql.org/docs/13/runtime-config-query.html#GUC-ENABLE-INCREMENTAL-SORT)）外，还为不同大小的负载提供了更好的数据管理体验。此版本针对日常管理进行了优化，为应用程序开发人员提供了更多便利，并增强了安全性。

“PostgreSQL 13展示了我们全球社区在增强世界上最先进的开源关系数据库功能方面的协作和奉献精神。”, PostgreSQL核心团队成员Peter Eisentraut说, “每个发行版所带来的创新以及其在可靠性和稳定性方面的声誉，这是为什么越来越多的人选择在其应用程序中使用PostgreSQL的原因”。

[PostgreSQL](https://www.postgresql.org)是一种创新的数据管理系统，以其可靠性和健壮性著称，得益于[全球开发者社区](https://www.postgresql.org/community/) 超过25年的开源开发，它已成为各种规模组织首选的开源关系型数据库。

### 持续的性能提升

在先前PostgreSQL版本的基础上，PostgreSQL 13可以有效地处理标准数据库索引[B-tree中的重复数据](https://www.postgresql.org/docs/13/btree-implementation.html#BTREE-DEDUPLICATION)。 这降低了B-tree索引所需的总体使用空间，同时提高了整体查询性能。

PostgreSQL 13引入了增量排序，其中查询中来自较早步骤的已排序数据可以加快后续步骤的排序。此外，PostgreSQL现在可以使用[扩展的统计信息](https://www.postgresql.org/docs/13/planner-stats.html#PLANNER-STATS-EXTENDED)（通过[`CREATE STATISTICS`](https://www.postgresql.org/docs/13/sql-createstatistics.html)访问）来创建增强带有`OR`子句和列表中的`IN`/`ANY`查找的查询计划。

在PostgreSQL 13中，更多类型的[聚合](https://www.postgresql.org/docs/13/functions-aggregate.html)和[分组](https://www.postgresql.org/docs/13/queries-table-expressions.html#QUERIES-GROUPING-SETS)可以利用PostgreSQL的高效哈希聚合功能，因为具有大聚合的查询不必完全放在内存中。 带有[分区表](https://www.postgresql.org/docs/13/ddl-partitioning.html)的查询性能得到了提高，因为现在有更多情况可以修剪分区并且可以直接连接分区。

### 管理优化

[清理(Vacuuming)](https://www.postgresql.org/docs/13/routine-vacuuming.html)是PostgreSQL管理的重要部分，它使数据库能够在更新和删除行之后回收存储空间。 尽管之前的PostgreSQL版本已经完成了减轻清理开销的工作，但是清理过程也可能带来管理上的挑战。

PostgreSQL 13通过引入[索引的并行清理](https://www.postgresql.org/docs/13/sql-vacuum.html)来继续改进清理系统。除了它提供的清理性能优势外，由于管理员可以选择要运行的并行Worker进程的数量，因此可以针对特定工作负载调整此新功能的使用。除了这些性能带来的好处之外，数据插入现在还可以触发自动清理过程。

[复制槽(Replication slots)](https://www.postgresql.org/docs/13/warm-standby.html#STREAMING-REPLICATION-SLOTS)用于防止预写日志（WAL）在备库收到之前被删除，可以在PostgreSQL 13中进行调整以指定[要保留的WAL文件的最大数量](https://www.postgresql.org/docs/13/runtime-config-replication.html#GUC-MAX-SLOT-WAL-KEEP-SIZE)，并有助于避免磁盘空间不足的错误。

PostgreSQL 13还增加了更多管理员可以监视数据库活动的方式，包括从`EXPLAIN`查看WAL使用情况的统计信息，基于流的备份进度，以及`ANALYZE`命令的进度。
另外，还可以使用新的[`pg_verifybackup`](https://www.postgresql.org/docs/13/app-pgverifybackup.html)命令来检查[`pg_basebackup`](https://www.postgresql.org/docs/13/app-pgbasebackup.html)命令输出的完整性。

### 便利的应用程序开发

PostgreSQL 13让使用来自不同数据源的PostgreSQL数据类型更加容易。此版本在SQL/JSON路径支持中添加了[`datetime()`](https://www.postgresql.org/docs/13/functions-json.html#FUNCTIONS-SQLJSON-OP-TABLE)函数，该函数将有效的时间格式（例如ISO 8601字符串）转换为PostgreSQL本地类型。
此外，UUID v4 生成函数[`gen_random_uuid()`](https://www.postgresql.org/docs/13/functions-uuid.html)现在可以直接使用而无需安装任何扩展。

PostgreSQL的分区系统更加灵活，因为分区表完全支持逻辑复制和BEFORE行级触发器。

PostgreSQL 13中的[`FETCH FIRST`](https://www.postgresql.org/docs/13/sql-select.html#SQL-LIMIT)语法现已扩展为可包含`WITH TIES`子句。 指定时，`WITH TIES`包括基于`ORDER BY`子句的结果集中最后一行相匹配的任何其他行。

### 安全增强

PostgreSQL的扩展系统是其强大功能的关键组成部分，因为它允许开发人员扩展其功能。在以前的版本中，新的扩展只能由数据库超级用户安装。为了更轻松地利用PostgreSQL的可扩展性，PostgreSQL 13添加了"[可信扩展](https://www.postgresql.org/docs/13/sql-createextension.html)"的概念，该概念允许数据库用户使用安装超级用户标记为“受信任”的扩展。某些内置扩展默认情况下标记为受信任，包括 [`pgcrypto`](https://www.postgresql.org/docs/13/pgcrypto.html), [`tablefunc`](https://www.postgresql.org/docs/13/tablefunc.html), [`hstore`](https://www.postgresql.org/docs/13/hstore.html)等。

对于需要安全身份验证方法的应用程序，PostgreSQL 13允许客户端在使用[SCRAM身份验证](https://www.postgresql.org/docs/13/sasl-authentication.html#SASL-SCRAM-SHA-256)时[要求通道绑定](https://www.postgresql.org/docs/13/libpq-connect.html#LIBPQ-CONNECT-CHANNEL-BINDING)，并且PostgreSQL外部数据包装器([`postgres_fdw`](https://www.postgresql.org/docs/13/postgres-fdw.html))现在可以使用基于证书的身份验证。

### 关于PostgreSQL

[PostgreSQL](https://www.postgresql.org)是世界上最先进的开源数据库，它的全球社区是一个由成千上万的用户、开发人员、公司或其他组织组成的。PostgreSQL起源于加利福尼亚大学伯克利分校，已经有30多年的历史，并且以无与伦比的开发速度继续发展。 PostgreSQL的成熟功能不仅与顶级商业数据库系统匹配，而且在高级数据库功能、可扩展性、安全性和稳定性方面超过了它们。

### 新闻稿翻译

* Grant Zhou, Cary Huang and David Zhang from Highgo Software

### 链接

* [下载](https://www.postgresql.org/download/)
* [发行说明](https://www.postgresql.org/docs/13/release-13.html)
* [新闻资料](https://www.postgresql.org/about/press/)
* [安全](https://www.postgresql.org/support/security/)
* [版本政策](https://www.postgresql.org/support/versioning/)
* [在Twitter上关注@postgresql](https://twitter.com/postgresql)