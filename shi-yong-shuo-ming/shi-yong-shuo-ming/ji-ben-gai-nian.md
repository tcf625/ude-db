# 基本概念

## 底階 API

### PersistenceContext  

對應實際資料來源，如 DataSource / SessionFactory，視選用底層而定。
* 可取得原始 JDBC Connection。(需自行注意資源管理)
* 可取得 SqlSelector, SqlExecutor 等基本操作類別。

### SqlSelector

提供 queryXXXX(....) 等一系列的查詢函式。

### SqlExecutor

提供 insert/update 函式執行 SQL 敘述，使用 insert 函式可取得更多的回應結果，如insert時自動建立的鍵值。

``` java 
InsertResult insert(String)
InsertResult insert(String, QueryParams)
InsertResult insertBatch(String, List<QueryParams>)
int update(String)
int update(String, QueryParams)
int updateBatch(String, List<QueryParams>)
```

## 高階 API 

### DBFacade

UDE 中操作資料庫的主要元件。資料增刪改查的操作入口。
包含 SqlSelector / SqlExecutor 可執行的操作，以及 OR Mapping 的相關操作。
 
詳見「資料操作」相關說明。

### DBFacadeFactory

建立DBFacade 的工廠類別，一般會以 Spring 自動注入此元件。

## 其它

常見於各函式傳入、回傳的 JAVA 類別，詳見「資料查詢」相關說明。

* 查詢參數：QueryParam & QueryParams
* 查詢參數(分頁)：PageParams
* 查詢條件：WhereBuilder
* 查詢結果：DBRowMap
* 查詢結果集合：PagedList / PageInfo




