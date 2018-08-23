## QuerySqlExecutor

利用 QuerySqlExecutor 組合 SQL，並執行查詢。

``` java
public interface JDBCQueryExecutor {
    PagedQueryResults executeQuery(final DBFacade dbFacade, 
            final PageParams pageParams);
            
    default PagedQueryResults executeQuery(final DBFacade dbFacade) {
        return this.executeQuery(dbFacade, PageParams.LIST_ALL);
    }
    default <T> PagedList<T> executeQuery(final DBFacade dbFacade, 
           final MapConverter<T> converter) {
        return this.executeQuery(dbFacade, converter, PageParams.LIST_ALL);
    }
    default <T> PagedList<T> executeQuery(final DBFacade dbFacade, 
           final MapConverter<T> converter, 
           final PageParams pageParams) {
        return converter.convertToObjects(this.executeQuery(dbFacade, pageParams));
    }
    default Optional<DBRowMap> executeQueryFirstRow(final DBFacade dbFacade) {
        final PagedQueryResults results = this.executeQuery(dbFacade, PageParams.FIRST_ROW);
        return Optional.ofNullable(UdeListUtils.get(results, 0));
    }
}

```


### 'From' 子句

#### 針對單一 TABLE 的查詢

靜態建構方法

``` java
  final QuerySqlExecutor se = QuerySqlExecutor.fromTable(tableName);
```

或是之後取得 TableBuilder

``` java   
  final QuerySqlExecutor se = new QuerySqlExecutor();
  se.getTableBuilder().add(tableName);  
```

#### TABLE Join

``` java 
final Table tableA = tableBuilder.add(subQuery, "a");
tableA.joinWithAlias("nccd1000", "b", Table.joinUsing("nationality", "code")); 
```

SQL : 

``` sql 
FROM (  SELECT STAT
        , GENDER
        , REASON
        , NATIONALITY
     FROM NCDF0003
    WHERE (REG_DATE >= ? AND REG_DATE <= ?)
      AND NATIONALITY <> '000' )  AS A JOIN NCCD1000 AS B ON A.NATIONALITY=B.CODE
```


### 'Columns' 子句
```
final SimpleQueryExecutor qe = new SimpleQueryExecutor();
// SELECT
qe.addColumn("b.code_g1");//
qe.addColumnWithAlias("SUM(case when a.gender='1' then stat else 0 end)", "column02");//
qe.addColumnWithAlias("SUM(case when a.gender='2' then stat else 0 end)", "column03");//
```


### 'Where' / 'Order By' / 'Group By'  子句

使用 WhereClauses 。


