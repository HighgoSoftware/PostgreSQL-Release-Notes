2022年10月6日 - PostgreSQL全球开发组今天宣布[PostgreSQL 15](https://www.postgresql.org/docs/15/release-15.html) 正式发布, 这是世界上[最先进的开源数据库](https://www.postgresql.org/) 的最新版本。

PostgreSQL 15 版本的发布主要侧重于性能提升上，在管理本地和分布式部署中的工作负载方面成效显著，包括改进的排序功能。此版本通过添加流行的 [`MERGE`](https://www.postgresql.org/docs/15/sql-merge.html) 命令改善了开发人员的体验，并添加了更多用于监测数据库状态的能力。

PostgreSQL核心组成员Jonathan Katz表示：“PostgreSQL开发者社区持续构建那些改善开发人员体验，并简化那些支持高性能数据工作负载的功能， PostgreSQL 15 展示了如何通过开放式软件开发，为我们的用户提供一个非常适合应用程序开发并保证其关键数据安全的数据库。”

[PostgreSQL](https://www.postgresql.org) 是一个创新的数据管理系统，以其可靠性和健壮性著称，得益于[全球开发者社区](https://www.postgresql.org/community/) 超过25年的开源开发，它已成为各种规模组织首选的开源关系型数据库。

### 排序及压缩性能提升

在这个最新版本中，PostgreSQL 改进了其内存和磁盘[排序](https://www.postgresql.org/docs/15/queries-order.html) 算法，基准测试显示，在对不同数据类型的排序时，速度可提高25%到400%不等 。使用 `row_number()`、`rank()`、`dense_rank()` 和 `count()` 作为[窗口函数](https://www.postgresql.org/docs/15/functions-window.html) 在 PostgreSQL 15 中也有性能上的优化。使用 [`SELECT DISTINCT`](https://www.postgresql.org/docs/15/queries-select-lists.html#QUERIES-DISTINCT)  的查询现在可以[并行执行](https://www.postgresql.org/docs/15/parallel-query.html) 。

基于[之前PostgreSQL版本](https://www.postgresql.org/about/press/presskit14/) 的异步远程查询功能，[PostgreSQL外部数据包装器](https://www.postgresql.org/docs/15/postgres-fdw.html) ，[`postgres_fdw`](https://www.postgresql.org/docs/15/postgres-fdw.html) ，现在可支持[异步提交](https://www.postgresql.org/docs/15/postgres-fdw.html#id-1.11.7.47.11.7) 。

PostgreSQL 15 的性能改进也扩展到了归档和备份工具。 PostgreSQL 15 增加了对 [预写日志(WAL) 文件](https://www.postgresql.org/docs/15/runtime-config-wal.html#GUC-WAL-COMPRESSION) LZ4和Zstandard (zstd)的压缩支持，这可以在一定的工作负载下获得空间和性能上的改进。在一些操作系统上，PostgreSQL 15 增加了对[WAL页面的预载支持](https://www.postgresql.org/docs/15/runtime-config-wal.html#GUC-RECOVERY-PREFETCH) 以帮助加快恢复时间。 PostgreSQL内置备份命令[`pg_basebackup`](https://www.postgresql.org/docs/15/app-pgbasebackup.html) ，现在支持服务器端的备份文件压缩，可以选择gzip、LZ4和zstd格式。 PostgreSQL 15 包含了使用[自定义模块进行归档](https://www.postgresql.org/docs/15/archive-modules.html) 的能力，从而减少了使用 shell 命令的开销。


### 开发人员特色功能

PostgreSQL 15 包含 SQL 标准的 [`MERGE`](https://www.postgresql.org/docs/15/sql-merge.html) 命令。`MERGE`允许用户编写包含 `INSERT`、`UPDATE`和`DELETE`操作的SQL语句。

最新版本增加了[使用正则表达式的新函数](https://www.postgresql.org/docs/15/functions-matching.html#FUNCTIONS-POSIX-REGEXP) 来检查字符串： `regexp_count()`, `regexp_instr()`, `regexp_like()`，和 `regexp_substr()`。 PostgreSQL 15还扩展了 `range_agg` 函数来聚合[上一个版本](https://www.postgresql.org/about/press/presskit14/) 引入的 [`multirange` 数据类型](https://www.postgresql.org/docs/15/rangetypes.html) 。

PostgreSQL 15 允许用户[使用访客权限而不是视图创建者权限创建视图](https://www.postgresql.org/docs/15/sql-createview.html) 。这个选项被称为 `security_invoker`，它增加了一层额外的保护，以确保视图调用者使用正确权限处理底层数据。

### 更多逻辑复制选项

PostgreSQL 15 为管理[逻辑复制](https://www.postgresql.org/docs/15/logical-replication.html) 提供了更多的灵活性。这个版本为[Publishers](https://www.postgresql.org/docs/15/logical-replication-publication.html)引入了[行筛选](https://www.postgresql.org/docs/15/logical-replication-row-filter.html)和[数据列列表](https://www.postgresql.org/docs/15/logical-replication-col-lists.html) 来允许用户选择从表中复制数据的子集。PostgreSQL 15 增加了一些功能来简化[冲突管理](https://www.postgresql.org/docs/15/logical-replication-conflicts.html) ，包括跳过重新执行冲突事务的能力，以及在检测到错误时自动停止订阅的能力。该版本还支持在逻辑复制中使用两阶段提交(2PC)。

### 日志和配置增强

PostgreSQL 15 引入了一种新的日志格式:[`jsonlog`](https://www.postgresql.org/docs/15/runtime-config-logging.html#RUNTIME-CONFIG-LOGGING-JSONLOG) 。 这种新格式使用JSON结构输出日志数据，这允许在结构化日志系统中处理PostgreSQL日志。

该版本在管理PostgreSQL配置方面为数据库管理员提供了更大的灵活性，增加了授予用户更改服务器级配置参数的权限的能力。此外，用户现在可以使用[`psql`](https://www.postgresql.org/docs/15/app-psql.html) 命令行工具中的`\dconfig`命令搜索有关配置的信息。

### 其他值得关注的改动

PostgreSQL[服务器级的统计数据](https://www.postgresql.org/docs/15/monitoring-stats.html)现在收集到共享内存，去除了统计收集进程以及定期将这些数据写入磁盘的过程。

PostgreSQL 15 使[ICU 排序](https://www.postgresql.org/docs/15/collation.html) 作为集群或单个数据库的默认排序规则成为可能。

该版本还增加了一个新的内置扩展[`pg_walinspect`](https://www.postgresql.org/docs/15/pgwalinspect.html) ，它允许用户直接从SQL接口检查预写日志文件的内容。

PostgreSQL 15 还允许除数据库所有者之外，从 `public` (或default)模式的数据库中 [撤销所有用户的`CREATE`权限](https://www.postgresql.org/docs/15/ddl-schemas.html#DDL-SCHEMAS-PATTERNS) 。

PostgreSQL 15 删除了长期被弃用的“独占备份”模式，也删除了PL/Python中对Python 2的支持。

### 关于PostgreSQL 

[PostgreSQL](https://www.postgresql.org) 是世界上最先进的开源数据库，拥有数以千计的用户、贡献者、公司和组织的全球社区。从加州大学伯克利分校开始，PostgreSQL建立在超过35年的工程基础上，一直延续着无与伦比的发展速度。PostgreSQL成熟的特性集不仅与顶级专有数据库系统相匹配，而且在高级数据库特性、可扩展性、安全性和稳定性方面都超过了顶级专有数据库系统。

### 链接

* [下载](https://www.postgresql.org/download/)
* [发行说明](https://www.postgresql.org/docs/15/release-15.html)
* [新闻资料](https://www.postgresql.org/about/press/)
* [安全页](https://www.postgresql.org/support/security/)
* [版本政策](https://www.postgresql.org/support/versioning/)
* [在Twitter上关注@postgresql](https://twitter.com/postgresql)

## 更多关于功能的信息

有关上述功能和其他功能的解释，请参阅以下资源:

* [发行说明](https://www.postgresql.org/docs/15/release-15.html)
* [详情参考](https://www.postgresql.org/about/featurematrix/)

## 下载地址

有几种方法可以下载PostgreSQL 15，包括:

* [官方下载](https://www.postgresql.org/download/) 页， 包括[Windows](https://www.postgresql.org/download/windows/) ，[Linux](https://www.postgresql.org/download/) ，[macOS](https://www.postgresql.org/download/macosx/) 等平台下的安装程序和工具
* [源代码](https://www.postgresql.org/ftp/source/v15.0)

其他工具和扩展可在[PostgreSQL Extension Network](http://pgxn.org/) 查看。

## 文档

PostgreSQL 15 提供了HTML文档和手册页，您还可以在线浏览 [HTML](https://www.postgresql.org/docs/15/) 和[PDF](https://www.postgresql.org/files/documentation/pdf/15/postgresql-15-US.pdf) 格式的文档。

## 许可证

PostgreSQL使用[PostgreSQL License](https://www.postgresql.org/about/licence/) ，一种类似bsd的“许可”License。这个[OSI认证的license](http://www.opensource.org/licenses/postgresql/) 因其灵活和业务友好而受到广泛赞赏，因为它不限制在商业和专有应用程序中使用PostgreSQL。 加上多公司的支持和代码的公共所有权，我们的许可证使得PostgreSQL非常受欢迎，因为供应商希望在自己的产品中嵌入数据库，而不必担心费用、供应商锁定或许可证条款的更改。

## 联系方式

网址

* [https://www.postgresql.org/](https://www.postgresql.org/)

邮箱

* [press@postgresql.org](mailto:press@postgresql.org)

## 图片和标志

Postgres和PostgreSQL以及大象标志(Slonik)都是[加拿大PostgreSQL社区协会](https://www.postgres.ca)注册的商标。如果您希望使用这些标志，您必须遵守[商标政策](https://www.postgresql.org/about/policies/trademarks/) 。

## 公司支持

PostgreSQL得到了许多公司的支持，他们赞助开发人员，提供托管资源，并给予我们财政支持。请查看我们的[赞助人](https://www.postgresql.org/about/sponsors/) 页面，了解这些项目的支持者。

也有大量[提供PostgreSQL支持的公司](https://www.postgresql.org/support/professional_support/) ，从个人顾问到跨国公司。

如果您希望为PostgreSQL全球发展小组或一个公认的社区非营利组织作出经济贡献，请访问我们的[捐赠](https://www.postgresql.org/about/donate/)页面 。 