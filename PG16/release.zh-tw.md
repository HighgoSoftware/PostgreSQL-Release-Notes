2023年9月14日 - PostgreSQL全球開發組今天宣佈PostgreSQL 16正式發佈，作為世界上最先進的開源數據庫，PostgreSQL 16是目前的最新版本。

[PostgreSQL 16](https://www.postgresql.org/docs/16/release-16.html)提升了性能，尤其在併行查詢、大數據量加載和邏輯復制方面有顯著的改進。該版本為開發人員和管理員提供了許多新功能，包括更多的SQL/JSON語法、新的工作負載監控指標，以及大型集群間更靈活的訪問控制規則定義。

"隨著關繫數據庫模式的發展，PostgreSQL繼續提升在查詢和處理大規模數據方面的性能"，PostgreSQL核心團隊成員Dave Page說，"PostgreSQL 16為用戶提供了更多縱嚮擴展（scale-up）和橫嚮擴展（scale-out）工作負載的方法，同時也為他們提供了理解和優化數據管理的新途徑。"

PostgreSQL是一個創新的數據管理繫統，以其可靠性和健壯性著稱，得益於全球開發者社區超過25年的開源開發，已經成為各種規模組織的首選開源關繫型數據庫。

### 性能提升

PostgreSQL 16通過新的查詢規劃器優化提升了現有PostgreSQL功能的性能。在該最新版本中，[查詢規劃器可以併行執行](https://www.postgresql.org/docs/16/parallel-query.html)`FULL`和 `RIGHT`[連接](https://www.postgresql.org/docs/16/queries-table-expressions.html#QUERIES-JOIN)，為使用帶有`DISTINCT`或`ORDER BY`子句的[聚合函數](https://www.postgresql.org/docs/16/functions-aggregate.html)的查詢生成更優的執行計劃，利用增量排序來處理[`SELECT DISTINCT`](https://www.postgresql.org/docs/16/queries-select-lists.html#QUERIES-DISTINCT)查詢，併優化[窗口函數](https://www.postgresql.org/docs/16/sql-expressions.html#SYNTAX-WINDOW-FUNCTIONS)，使其執行更加高效。它還改進了`RIGHT`和`OUTER`“反連接（anti-joins）”，使用戶能夠識別出不在已連接錶中的數據行。

該版本包含單一和併發操作中使用[`COPY`](https://www.postgresql.org/docs/16/sql-copy.html)進行批量加載的改進，測試顯示在某些情況下性能提升高達300%。PostgreSQL 16增加了對使用libpq的客戶端的[負載均衡](https://www.postgresql.org/docs/16/libpq-connect.html#LIBPQ-CONNECT-LOAD-BALANCE-HOSTS)支持，併改進了vacuum策略，減少全錶凍結的必要性。此外，PostgreSQL 16引入了在x86和ARM架構上使用 `SIMD` 的CPU加速，從而在處理ASCII和JSON字符串以及執行數組和子事務搜索時，性能有所提升。

### 邏輯復制

[邏輯復制](https://www.postgresql.org/docs/16/logical-replication.html)允許用戶將數據流復制到其他可以解析PostgreSQL邏輯復制協議的節點或訂閱者。在PostgreSQL 16中，用戶可以從備節點（standby）執行邏輯復制，這意味著備節點可以將邏輯變更發佈到其他服務器。這為開發者提供了新的工作負載分佈選項——例如，使用備節點而不是更繁忙的主節點通過邏輯復制將更改應用到下級訂閱端。

此外，PostgreSQL 16中對邏輯復制進行了多項性能改進。訂閱者現在可以使用併行方式來處理大型事務。對於沒有[主鍵](https://www.postgresql.org/docs/16/ddl-constraints.html#DDL-CONSTRAINTS-PRIMARY-KEYS)的錶，訂閱者可以使用B-tree索引而不是順序掃描來查找行。在某些條件下，用戶還可以使用二進制格式加速初始錶同步。

PostgreSQL 16邏輯復制的訪問控制做了多項改進，包括新的[預定義角色](https://www.postgresql.org/docs/16/predefined-roles.html) `pg_create_subscription`，該角色允許用戶新建邏輯訂閱。

該版本開始支持雙嚮邏輯復制功能，可以在兩個不同發佈者的錶之間進行數據復制。

### 開發者體驗

PostgreSQL 16 添加了更多[SQL/JSON](https://www.postgresql.org/docs/16/functions-json.html)標准的語法，包括構造函數和謂詞，比如 `JSON_ARRAY()`、`JSON_ARRAYAGG()` 和 `IS JSON`。該版本允許使用下劃線作為仟位分隔符（例如 `5_432_000`），併支持非十進制整數常量（如 `0x1538`、`0o12470`和`0b1010100111000`）。

PostgreSQL 16 為開發者提供更多 `psql` 命令，包括[`\bind`](https://www.postgresql.org/docs/16/app-psql.html#APP-PSQL-META-COMMAND-BIND)，該命令允許用戶使用帶參數的查詢，併使用 `\bind` 來代替變量（例如 `SELECT $1::int + $2::int \bind 1 2 \g`）。

PostgreSQL 16 對規定如何排序文本的[文本排序規則（text collations）](https://www.postgresql.org/docs/16/collation.html)進行了改進。PostgreSQL 16構建（Build）時默認啟用ICU（國際化組件），併從繫統環境中確定默認的ICU區域設置，允許用戶自定義ICU排序規則。

### 監控

理解I/O操作對繫統的影響是優化數據庫工作負載性能的一個關鍵方面。PostgreSQL 16 引入了一項與I/O操作相關的關鍵性新指標[`pg_stat_io`](https://www.postgresql.org/docs/16/monitoring-stats.html#MONITORING-PG-STAT-IO-VIEW)，用於詳細分析I/O訪問模式。

此外，該版本在[`pg_stat_all_tables`](https://www.postgresql.org/docs/16/monitoring-stats.html#MONITORING-PG-STAT-ALL-TABLES-VIEW)視圖中添加了一個新字段，該字段記錄了最後一次掃描錶或索引的時間戳。PostgreSQL 16通過記錄語句中傳進來的參數值，提升了[`auto_explain`](https://www.postgresql.org/docs/16/auto-explain.html)的可讀性，以及[`pg_stat_statements`](https://www.postgresql.org/docs/16/pgstatstatements.html)和[`pg_stat_activity`](https://www.postgresql.org/docs/16/monitoring-stats.html#MONITORING-PG-STAT-ACTIVITY-VIEW)使用查詢跟蹤算法的准確性。

### 訪問控制與安全性

PostgreSQL 16 提供了更精細的訪問控制選項，併增強了相關安全功能。該版本對[`pg_hba.conf`](https://www.postgresql.org/docs/16/auth-pg-hba-conf.html)和[`pg_ident.conf`](https://www.postgresql.org/docs/16/auth-username-maps.html)的管理做了改進，包括允許使用正則錶達式匹配用戶和數據庫名稱，併支持使用`include`指令來引入外部配置文件。

該版本添加了幾個有關安全性的客戶端連接參數，包括`require_auth`，它允許客戶端指定可接受的來自服務器端的身份驗證參數，以及[`sslrootcert="system"`](https://www.postgresql.org/docs/16/libpq-connect.html#LIBPQ-CONNECT-SSLROOTCERT)，該參數錶示PostgreSQL將使用客戶端操作繫統提供的可信證書（CA）。此外，該版本增加了對 Kerberos 信任委托的支持，允許諸如 [`postgres_fdw`](https://www.postgresql.org/docs/16/postgres-fdw.html) 和 [`dblink`](https://www.postgresql.org/docs/16/dblink.html) 這樣的擴展（extension）使用經過身份驗證的憑證連接到受信任的服務。

### 關於PostgreSQL

[PostgreSQL](https://www.postgresql.org) 是全球最先進的開源數據庫，它的全球社區是一個擁有數以仟計的用戶、貢獻者、公司和組織組成的。PostgreSQL起源於加利福尼亞大學伯克利分校，已經有超過35年的歴史，併且以無與倫比的速度持續發展。PostgreSQL成熟的特性不僅與頂級商業數據庫繫統匹配，而且在高級數據庫功能、可擴展性、安全性和穩定性方面超過了它們。

### 鏈接

* [下載](https://www.postgresql.org/download/)
* [發行說明](https://www.postgresql.org/docs/16/release-16.html)
* [新聞資料](https://www.postgresql.org/about/press/)
* [安全](https://www.postgresql.org/support/security/)
* [版本政策](https://www.postgresql.org/support/versioning/)
* [在Twitter上關註@postgresql](https://twitter.com/postgresql)
* [捐贈](https://www.postgresql.org/about/donate/)

## 更多功能信息

有關上述功能和其他信息的解釋，請參考以下資源:

* [發行說明](https://www.postgresql.org/docs/16/release-16.html)
* [功能列錶](https://www.postgresql.org/about/featurematrix/)

## 下載地址

您可以通過多種方式下載PostgreSQL 16，包括：

* [官方下載](https://www.postgresql.org/download/)頁面，包含用於[Windows](https://www.postgresql.org/download/windows/)、[Linux](https://www.postgresql.org/download/linux/)、[macOS](https://www.postgresql.org/download/macosx/) 等多個平臺的安裝程序和工具。
* [源代碼](https://www.postgresql.org/ftp/source/v16.0)

其他工具和擴展可在[PostgreSQL Extension Network](http://pgxn.org/)上找到。

## 文檔

PostgreSQL 16 附帶了HTML文檔和手冊，您還可以在線瀏覽[HTML](https://www.postgresql.org/docs/16/)和[PDF](https://www.postgresql.org/files/documentation/pdf/16/postgresql-16-US.pdf)格式的文檔。

## 許可證

PostgreSQL使用[PostgreSQL 許可證](https://www.postgresql.org/about/licence/)，這是一個類似 BSD 的“寬鬆”許可證。這個[OSI認證的許可證](http://www.opensource.org/licenses/postgresql/) 因其靈活性和適用於商業環境而受到廣泛贊譽，因為它不限制在商業和專有應用程序中使用PostgreSQL。加上多公司支持和代碼的公共所有權，該許可證使PostgreSQL非常受歡迎，因為供應商希望在自己的產品中嵌入數據庫，而無需擔心費用、供應商鎖定或許可條款變更。

## 聯繫方式

網址

* [https://www.postgresql.org/](https://www.postgresql.org/)

郵箱

* [press@postgresql.org](mailto:press@postgresql.org)

## 圖像和標誌

Postgres、PostgreSQL和大象標誌（Slonik）都是[PostgreSQL 社區協會](https://www.postgres.ca)的註冊商標。如果您希望使用這些標誌，您必須遵守[商標政策](https://www.postgresql.org/about/policies/trademarks/)。

## 企業支持

PostgreSQL得到了許多公司的支持，他們贊助開發人員，提供托管資源，併給予我們財務支持。請查看我們的[贊助商](https://www.postgresql.org/about/sponsors/)頁面，了解這些項目的支持者。

還有大量[提供PostgreSQL支持的公司](https://www.postgresql.org/support/professional_support/) ，包括個人顧問到跨國公司。

如果您希望對PostgreSQL全球開發組或其中一個公認的社群非營利組織進行捐贈，請訪問我們的[捐贈](https://www.postgresql.org/about/donate/)頁面。
