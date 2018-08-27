#### ~SqlExecutor

利用 ~SqlExecutor 組合 SQL，並執行操作。

* UpdateSqlExecutor  extends WhereBuilderWrapper 

```
setByClause(ColumnDefine<?>, String)  // 直接傳入 SQL 字句做為被設定的值。 
setByClause(String, String)           // 直接傳入 SQL 字句做為被設定的值。 
setByParam(ColumnDefine<?>, int)
setByParam(ColumnDefine<?>, QueryParam)
setByParam(ColumnDefine<?>, String)
setByParam(String, QueryParam)
setByParam(String, String)
setNull(ColumnDefine<?>)
setNull(String)

updateTable(DBFacade, String)
extraClause(ExtraClauseOP, String) // UnsupportedOperationException 不能再加上 group/order by 
```

* DeleteSqlExecutor  extends WhereBuilderWrapper 

```
deleteTable(DBFacade, String)
extraClause(ExtraClauseOP, String) // UnsupportedOperationException
```

* InsertSqlExecutor 

``` java
insertByClause(ColumnDefine<?>, String clause) // 直接傳入 SQL 字句做為被設定的值。 
insertByClause(String, String clause)          //

insertByParam(ColumnDefine<?>, QueryParam)
insertByParam(ColumnDefine<?>, String)
insertByParam(String, QueryParam)
insertByParam(String, String)

insertTable(DBFacade) // 執行 INSERT 操作.
```
