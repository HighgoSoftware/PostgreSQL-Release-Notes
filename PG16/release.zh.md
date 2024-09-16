2023年9月14日 - PostgreSQL全球开发组今天宣布PostgreSQL 16正式发布，作为世界上最先进的开源数据库，PostgreSQL 16是目前的最新版本。

[PostgreSQL 16](https://www.postgresql.org/docs/16/release-16.html)提升了性能，尤其在并行查询、大数据量加载和逻辑复制方面有显著的改进。该版本为开发人员和管理员提供了许多新功能，包括更多的SQL/JSON语法、新的工作负载监控指标，以及大型集群间更灵活的访问控制规则定义。

"随着关系数据库模式的发展，PostgreSQL继续提升在查询和处理大规模数据方面的性能"，PostgreSQL核心团队成员Dave Page说，"PostgreSQL 16为用户提供了更多纵向扩展（scale-up）和横向扩展（scale-out）工作负载的方法，同时也为他们提供了理解和优化数据管理的新途径。"

PostgreSQL是一个创新的数据管理系统，以其可靠性和健壮性著称，得益于全球开发者社区超过25年的开源开发，已经成为各种规模组织的首选开源关系型数据库。

### 性能提升

PostgreSQL 16通过新的查询规划器优化提升了现有PostgreSQL功能的性能。在该最新版本中，[查询规划器可以并行执行](https://www.postgresql.org/docs/16/parallel-query.html)`FULL`和 `RIGHT`[连接](https://www.postgresql.org/docs/16/queries-table-expressions.html#QUERIES-JOIN)，为使用带有`DISTINCT`或`ORDER BY`子句的[聚合函数](https://www.postgresql.org/docs/16/functions-aggregate.html)的查询生成更优的执行计划，利用增量排序来处理[`SELECT DISTINCT`](https://www.postgresql.org/docs/16/queries-select-lists.html#QUERIES-DISTINCT)查询，并优化[窗口函数](https://www.postgresql.org/docs/16/sql-expressions.html#SYNTAX-WINDOW-FUNCTIONS)，使其执行更加高效。它还改进了`RIGHT`和`OUTER`“反连接（anti-joins）”，使用户能够识别出不在已连接表中的数据行。

该版本包含单一和并发操作中使用[`COPY`](https://www.postgresql.org/docs/16/sql-copy.html)进行批量加载的改进，测试显示在某些情况下性能提升高达300%。PostgreSQL 16增加了对使用libpq的客户端的[负载均衡](https://www.postgresql.org/docs/16/libpq-connect.html#LIBPQ-CONNECT-LOAD-BALANCE-HOSTS)支持，并改进了vacuum策略，减少全表冻结的必要性。此外，PostgreSQL 16引入了在x86和ARM架构上使用 `SIMD` 的CPU加速，从而在处理ASCII和JSON字符串以及执行数组和子事务搜索时，性能有所提升。

### 逻辑复制

[逻辑复制](https://www.postgresql.org/docs/16/logical-replication.html)允许用户将数据流复制到其他可以解析PostgreSQL逻辑复制协议的节点或订阅者。在PostgreSQL 16中，用户可以从备节点（standby）执行逻辑复制，这意味着备节点可以将逻辑变更发布到其他服务器。这为开发者提供了新的工作负载分布选项——例如，使用备节点而不是更繁忙的主节点通过逻辑复制将更改应用到下级订阅端。

此外，PostgreSQL 16中对逻辑复制进行了多项性能改进。订阅者现在可以使用并行方式来处理大型事务。对于没有[主键](https://www.postgresql.org/docs/16/ddl-constraints.html#DDL-CONSTRAINTS-PRIMARY-KEYS)的表，订阅者可以使用B-tree索引而不是顺序扫描来查找行。在某些条件下，用户还可以使用二进制格式加速初始表同步。

PostgreSQL 16逻辑复制的访问控制做了多项改进，包括新的[预定义角色](https://www.postgresql.org/docs/16/predefined-roles.html) `pg_create_subscription`，该角色允许用户新建逻辑订阅。

该版本开始支持双向逻辑复制功能，可以在两个不同发布者的表之间进行数据复制。

### 开发者体验

PostgreSQL 16 添加了更多[SQL/JSON](https://www.postgresql.org/docs/16/functions-json.html)标准的语法，包括构造函数和谓词，比如 `JSON_ARRAY()`、`JSON_ARRAYAGG()` 和 `IS JSON`。该版本允许使用下划线作为千位分隔符（例如 `5_432_000`），并支持非十进制整数常量（如 `0x1538`、`0o12470`和`0b1010100111000`）。

PostgreSQL 16 为开发者提供更多 `psql` 命令，包括[`\bind`](https://www.postgresql.org/docs/16/app-psql.html#APP-PSQL-META-COMMAND-BIND)，该命令允许用户使用带参数的查询，并使用 `\bind` 来代替变量（例如 `SELECT $1::int + $2::int \bind 1 2 \g`）。

PostgreSQL 16 对规定如何排序文本的[文本排序规则（text collations）](https://www.postgresql.org/docs/16/collation.html)进行了改进。PostgreSQL 16构建（Build）时默认启用ICU（国际化组件），并从系统环境中确定默认的ICU区域设置，允许用户自定义ICU排序规则。

### 监控

理解I/O操作对系统的影响是优化数据库工作负载性能的一个关键方面。PostgreSQL 16 引入了一项与I/O操作相关的关键性新指标[`pg_stat_io`](https://www.postgresql.org/docs/16/monitoring-stats.html#MONITORING-PG-STAT-IO-VIEW)，用于详细分析I/O访问模式。

此外，该版本在[`pg_stat_all_tables`](https://www.postgresql.org/docs/16/monitoring-stats.html#MONITORING-PG-STAT-ALL-TABLES-VIEW)视图中添加了一个新字段，该字段记录了最后一次扫描表或索引的时间戳。PostgreSQL 16通过记录语句中传进来的参数值，提升了[`auto_explain`](https://www.postgresql.org/docs/16/auto-explain.html)的可读性，以及[`pg_stat_statements`](https://www.postgresql.org/docs/16/pgstatstatements.html)和[`pg_stat_activity`](https://www.postgresql.org/docs/16/monitoring-stats.html#MONITORING-PG-STAT-ACTIVITY-VIEW)使用查询跟踪算法的准确性。

### 访问控制与安全性

PostgreSQL 16 提供了更精细的访问控制选项，并增强了相关安全功能。该版本对[`pg_hba.conf`](https://www.postgresql.org/docs/16/auth-pg-hba-conf.html)和[`pg_ident.conf`](https://www.postgresql.org/docs/16/auth-username-maps.html)的管理做了改进，包括允许使用正则表达式匹配用户和数据库名称，并支持使用`include`指令来引入外部配置文件。

该版本添加了几个有关安全性的客户端连接参数，包括`require_auth`，它允许客户端指定可接受的来自服务器端的身份验证参数，以及[`sslrootcert="system"`](https://www.postgresql.org/docs/16/libpq-connect.html#LIBPQ-CONNECT-SSLROOTCERT)，该参数表示PostgreSQL将使用客户端操作系统提供的可信证书（CA）。此外，该版本增加了对 Kerberos 信任委托的支持，允许诸如 [`postgres_fdw`](https://www.postgresql.org/docs/16/postgres-fdw.html) 和 [`dblink`](https://www.postgresql.org/docs/16/dblink.html) 这样的扩展（extension）使用经过身份验证的凭证连接到受信任的服务。

### 关于PostgreSQL

[PostgreSQL](https://www.postgresql.org) 是全球最先进的开源数据库，它的全球社区是一个拥有数以千计的用户、贡献者、公司和组织组成的。PostgreSQL起源于加利福尼亚大学伯克利分校，已经有超过35年的历史，并且以无与伦比的速度持续发展。PostgreSQL成熟的特性不仅与顶级商业数据库系统匹配，而且在高级数据库功能、可扩展性、安全性和稳定性方面超过了它们。

### 链接

* [下载](https://www.postgresql.org/download/)
* [发行说明](https://www.postgresql.org/docs/16/release-16.html)
* [新闻资料](https://www.postgresql.org/about/press/)
* [安全](https://www.postgresql.org/support/security/)
* [版本政策](https://www.postgresql.org/support/versioning/)
* [在Twitter上关注@postgresql](https://twitter.com/postgresql)
* [捐赠](https://www.postgresql.org/about/donate/)

## 更多功能信息

有关上述功能和其他信息的解释，请参考以下资源:

* [发行说明](https://www.postgresql.org/docs/16/release-16.html)
* [功能列表](https://www.postgresql.org/about/featurematrix/)

## 下载地址

您可以通过多种方式下载PostgreSQL 16，包括：

* [官方下载](https://www.postgresql.org/download/)页面，包含用于[Windows](https://www.postgresql.org/download/windows/)、[Linux](https://www.postgresql.org/download/linux/)、[macOS](https://www.postgresql.org/download/macosx/) 等多个平台的安装程序和工具。
* [源代码](https://www.postgresql.org/ftp/source/v16.0)

其他工具和扩展可在[PostgreSQL Extension Network](http://pgxn.org/)上找到。

## 文档

PostgreSQL 16 附带了HTML文档和手册，您还可以在线浏览[HTML](https://www.postgresql.org/docs/16/)和[PDF](https://www.postgresql.org/files/documentation/pdf/16/postgresql-16-US.pdf)格式的文档。

## 许可证

PostgreSQL使用[PostgreSQL 许可证](https://www.postgresql.org/about/licence/)，这是一个类似 BSD 的“宽松”许可证。这个[OSI认证的许可证](http://www.opensource.org/licenses/postgresql/) 因其灵活性和适用于商业环境而受到广泛赞誉，因为它不限制在商业和专有应用程序中使用PostgreSQL。加上多公司支持和代码的公共所有权，该许可证使PostgreSQL非常受欢迎，因为供应商希望在自己的产品中嵌入数据库，而无需担心费用、供应商锁定或许可条款变更。

## 联系方式

网址

* [https://www.postgresql.org/](https://www.postgresql.org/)

邮箱

* [press@postgresql.org](mailto:press@postgresql.org)

## 图像和标志

Postgres、PostgreSQL和大象标志（Slonik）都是[PostgreSQL 社区协会](https://www.postgres.ca)的注册商标。如果您希望使用这些标志，您必须遵守[商标政策](https://www.postgresql.org/about/policies/trademarks/)。

## 企业支持

PostgreSQL得到了许多公司的支持，他们赞助开发人员，提供托管资源，并给予我们财务支持。请查看我们的[赞助商](https://www.postgresql.org/about/sponsors/)页面，了解这些项目的支持者。

还有大量[提供PostgreSQL支持的公司](https://www.postgresql.org/support/professional_support/) ，包括个人顾问到跨国公司。

如果您希望对PostgreSQL全球开发组或其中一个公认的社群非营利组织进行捐赠，请访问我们的[捐赠](https://www.postgresql.org/about/donate/)页面。
