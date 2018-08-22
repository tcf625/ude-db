# 基本概念

## PersistenceContext : 底階 API

對應實際資料來源，如 DataSource / SessionFactory，視選用底層而定。
* 可取得原始 JDBC Connection。(需自行注意資源管理)
* 可取得 SqlSelector, SqlExecutor 等基本操作類別。

### SqlSelector

提供 queryXXXX(....) 等一系列的查詢函式。

### SqlExecutor

提供 insert/update 函式執行 SQL 敘述，insert 函式可取得更多的回應結果，如insert時自動建立的鍵值。

``` java 
InsertResult insert(String)
InsertResult insert(String, QueryParams)
InsertResult insertBatch(String, List<QueryParams>)
int update(String)
int update(String, QueryParams)
int updateBatch(String, List<QueryParams>)
```

## DBFacade

UDE 中操作資料庫的主要元件。資料增刪改查的操作入口。

## DBFacadeFactory

建立DBFacade 的工廠類別，一般以 Spring 自動注入此元件。


* 詳見資料操作

## 其它

* QueryParam & QueryParams (見資料查詢)
* PageParams (見資料查詢)
* DBRowMap (見資料操作)
* PagedList / PageInfo (見資料操作)
* WhereBuilder (見資料查詢)



