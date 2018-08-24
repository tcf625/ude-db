#### ~SqlExecutor

利用 ~SqlExecutor 組合 SQL，並執行操作。

* UpdateSqlExecutor  extends WhereBuilderWrapper 

```
setByClause(ColumnDefine<?>, String)
setByClause(String, String)
setByParam(ColumnDefine<?>, int)
setByParam(ColumnDefine<?>, QueryParam)
setByParam(ColumnDefine<?>, String)
setByParam(String, QueryParam)
setByParam(String, String)
setNull(ColumnDefine<?>)
setNull(String)
updateTable(DBFacade, String)
extraClause(ExtraClauseOP, String) // UnsupportedOperationException
```

* DeleteSqlExecutor  extends WhereBuilderWrapper 

```
deleteTable(DBFacade, String)
extraClause(ExtraClauseOP, String) // UnsupportedOperationException
```

* InsertSqlExecutor 

```
InsertSqlExecutor(String)
insertByClause(ColumnDefine<?>, String)
insertByClause(String, String)
insertByParam(ColumnDefine<?>, QueryParam)
insertByParam(ColumnDefine<?>, String)
insertByParam(String, QueryParam)
insertByParam(String, String)
insertTable(DBFacade)
```
