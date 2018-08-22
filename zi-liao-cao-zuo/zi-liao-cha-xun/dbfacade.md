## DBFacade的查詢函式

### 多列查詢 (NativeSQL as DBRowMap)
  
  * 定義於 **SqlSelector** 介面。
  * 原則上查詢字串使用 NativeSQL ，除非選用支援 HQL/... 的底層實作。

* #### 回傳結果以 DBRowMap 表示

* 每一筆資料列的回傳結果以 **DBRowMap** 表示。
* \( PagedQueryResults extends PagedArrayList < DBRowMap\> \)

``` java
PagedQueryResults queryForList(String)
PagedQueryResults queryForList(String, QueryParams)
PagedQueryResults queryForList(String, QueryParams, PageParams)
```

* #### 自行定義資料轉換器

``` java
<T> PagedList<T> queryForList(MapConverter<T>, String)
<T> PagedList<T> queryForList(MapConverter<T>, String, QueryParams)
<T> PagedList<T> queryForList(MapConverter<T>, String, QueryParams, PageParams)

public interface MapConverter<T> {
    T convert(DBRowMap map);
}
```

## 多列查詢 (含 OR Mapping)


### * 以 '物件類型' 做 OR-Mapping 查詢

* Class<T> cls 決定查詢的 TABLE 對象。
* 查詢條件使用 WhereBuilder 帶入。

``` java
<T> PagedList<T> queryForList(Class<T> cls) 
<T> PagedList<T> queryForList(Class<T> cls, PageParams pageParams) 
<T> PagedList<T> queryForList(Class<T> cls, StringWhereBuilder whereBuilder) 
<T> PagedList<T> queryForList(Class<T> cls, StringWhereBuilder whereBuilder, PageParams pageParams);
```


### * 以 '查詢字串'、'查詢參數' 取得Query結果

* 試著以 Class<T> cls 對應的 MapConverter，轉換每列結果為 T

``` java
<T> PagedList<T> queryForList(Class<T>, String)
<T> PagedList<T> queryForList(Class<T>, String, QueryParams)
<T> PagedList<T> queryForList(Class<T>, String, QueryParams, PageParams)
```



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
<T> PagedList<T>  queryWithAction(RowAction<T>, String)
<T> PagedList<T>  queryWithAction(RowAction<T>, String, QueryParams)
<T> PagedList<T>  queryWithAction(RowAction<T>, String, QueryParams, PageParams)
```





