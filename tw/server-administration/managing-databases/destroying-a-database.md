# 22.5. 刪除資料庫Destroying a Database

資料庫可以透過指令[DROP DATABASE]進行刪除的動作 Databases are destroyed with the command [DROP DATABASE](https://www.postgresql.org/docs/13/sql-dropdatabase.html):

```
DROP DATABASE <資料庫名稱 >DROP DATABASE name;
```

只有資料庫的擁有者或超級使用者才可以執行刪除資料庫的動作. 刪除資料庫會同步清除所有資料庫內部的物件. 特別要注意的是一旦刪除資料庫的指令確認執行後便不可撤銷 Only the owner of the database, or a superuser, can drop a database. Dropping a database removes all objects that were contained within the database. The destruction of a database cannot be undone.

使用者不可以刪除正在連線的資料庫．使用者可以透連線到欲刪除資料庫對象以外的任何資料庫執行刪除指令，包含`template1`資料庫．`template1`也是刪除叢集中最後一個使用者資料庫的唯一連線選項．You cannot execute the `DROP DATABASE` command while connected to the victim database. You can, however, be connected to any other database, including the `template1` database. `template1` would be the only option for dropping the last user database of a given cluster.

For convenience, there is also a shell program to drop databases, [dropdb](https://www.postgresql.org/docs/13/app-dropdb.html):
為了方便起見, 我們可以透過執行shell程式來刪除資料庫, shell程式中所需的相關刪除資料庫指令選項, 請參照  [dropdb](https://www.postgresql.org/docs/14/app-dropdb.html):
```
dropdb dbname
```

(Unlike `createdb`, it is not the default action to drop the database with the current user name.)\\
(不同於新建資料庫指令, 刪除資料庫不是取決於目前的連線使用者; 必須是資料庫擁有者或超級使用者)
