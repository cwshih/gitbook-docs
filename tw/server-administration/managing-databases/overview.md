# 22.1. 概述

PostgreSQL叢集內部筆數較少的物件, 像是角色, 資料庫以及表格空間(tablespace)名稱等, 會被定義在叢集的級別並儲存在`pg_global`表格空間. 叢集是由多個資料庫所組成的, 資料庫間彼此獨立運用但可以共同存取叢集級別的物件. 每個資料庫是由多個綱要(schema)所組成, 綱要包含了像是表格與函數等物件. 所以一個PostgreSQL叢集的完整階層是: 叢集, 資料庫, 綱要, 表格 \(或是其他類型的物件, 像是函數\).

當連線到資料庫伺服器時, 使用者端在連線需求資訊中必須指明欲連線的資料庫名稱. 一個連線只能存取一個資料庫. 但使用者端可以透過多個連線需求存取相同或不同的資料庫. 資料庫級別的安全控管有兩種: 存取控制(access control)\(見 [第20.1節](https://www.postgresql.org/docs/13/auth-pg-hba-conf.html)\), 在連線時進行管控, 以及授權控制(authorization control) \(見 [第5.7節](https://www.postgresql.org/docs/13/ddl-priv.html)\), 透過系統權限授予進行控管. 外部資料包裝函式(Foreign data wrappers) \(見 [postgres\_fdw](https://www.postgresql.org/docs/13/postgres-fdw.html)\) 可以使自身資料庫內部的物件作為其他資料庫或叢集中物件的代理. 舊版的dblink 模組 \(見 [dblink](https://www.postgresql.org/docs/13/dblink.html)\) 提供了一個類似的功能. 在預設情況下, 所有使用者能夠透過所有連線方式去存取不同的資料庫.

If one PostgreSQL server cluster is planned to contain unrelated projects or users that should be, for the most part, unaware of each other, it is recommended to put them into separate databases and adjust authorizations and access controls accordingly. If the projects or users are interrelated, and thus should be able to use each other's resources, they should be put in the same database but probably into separate schemas; this provides a modular structure with namespace isolation and authorization control. More information about managing schemas is in [Section 5.9](https://www.postgresql.org/docs/13/ddl-schemas.html).

While multiple databases can be created within a single cluster, it is advised to consider carefully whether the benefits outweigh the risks and limitations. In particular, the impact that having a shared WAL \(see [Chapter 29](https://www.postgresql.org/docs/13/wal.html)\) has on backup and recovery options. While individual databases in the cluster are isolated when considered from the user's perspective, they are closely bound from the database administrator's point-of-view.

Databases are created with the `CREATE DATABASE` command \(see [Section 22.2](https://www.postgresql.org/docs/13/manage-ag-createdb.html)\) and destroyed with the `DROP DATABASE` command \(see [Section 22.5](https://www.postgresql.org/docs/13/manage-ag-dropdb.html)\). To determine the set of existing databases, examine the `pg_database` system catalog, for example

```text
SELECT datname FROM pg_database;
```

The [psql](https://www.postgresql.org/docs/13/app-psql.html) program's `\l` meta-command and `-l` command-line option are also useful for listing the existing databases.

#### Note

The SQL standard calls databases “catalogs”, but there is no difference in practice.

