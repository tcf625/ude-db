# DBFacade (查詢)


## 列表查詢 (NativeSQL as DBRowMap)

* 定義於 SqlSelector 介面。
* 每一筆資料列的回傳結果以 DBRowMap 表示。

queryForList(MapConverter<T>, String)
queryForList(MapConverter<T>, String, QueryParams)
queryForList(MapConverter<T>, String, QueryParams, PageParams)
queryForList(String)
queryForList(String, QueryParams)
queryForList(String, QueryParams, PageParams)

## 列表查詢 (含 OR Mapping)


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






