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


# 'From'

## 針對單一 TABLE 的查詢

''' java
  final QuerySqlExecutor se = QuerySqlExecutor.fromTable(tableName);
  
  
  final QuerySqlExecutor se = new QuerySqlExecutor();
  se.getTableBuilder().add(tableName);  
'''

## TABLE Join



# 'Columns'




# 'Where'



# 'Order By' / 'Group By' 