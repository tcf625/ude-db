## 單一列查詢

### 以主鍵為 criteria 查詢

```
T queryForObject(Class<T>, T)
```

### 取得回傳的第一筆資料列。

* 若無查詢結果，return Optional.empty()

```
Optional<DBRowMap> queryRowForMap(String)
Optional<DBRowMap> queryRowForMap(String, QueryParams)
```

## 欄位(Column)查詢


* 同 DbRowMap.getRawData() 結果

``` java
Object queryColumn(String)
Object queryColumn(String, QueryParams)
Object queryColumn(String, QueryParams, int)
// 自動嘗試 CAST，無法自動轉換時，回應 DB1104E("指定查詢資料類別有誤")
default <T> T queryColumn(final Class<T> cls, final String queryString) {
```

* 同 DbRowMap.get 結果

``` java
String queryColumnForString(String)
String queryColumnForString(String, QueryParams)
String queryColumnForString(String, QueryParams, int)
```


## 以 RowAction<T> 尋訪操作

* 如果RowAction 的 T process(final DBRowMap map) 回傳NULL，則結果不會加入LIST中。
* 適用於處理大量資料時機。

``` java
<T> PagedList<T> queryWithAction(RowAction<T>, String)
<T> PagedList<T> queryWithAction(RowAction<T>, String, QueryParams)
<T> PagedList<T> queryWithAction(RowAction<T>, String, QueryParams, PageParams)
```


