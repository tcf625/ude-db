# DBFacade (查詢)


## 列表查詢 (NativeSQL as DBRowMap)

* 定義於 **SqlSelector** 介面。


### * 回傳結果以 **DBRowMap** 表示

* 每一筆資料列的回傳結果以 **DBRowMap** 表示。
* \( PagedQueryResults extends PagedArrayList\<DBRowMap\> \)

``` java
PagedQueryResults queryForList(String)
PagedQueryResults queryForList(String, QueryParams)
PagedQueryResults queryForList(String, QueryParams, PageParams)
```

### * 自行定義資料轉換器

``` java
<T> PagedList<T> queryForList(MapConverter<T>, String)
<T> PagedList<T> queryForList(MapConverter<T>, String, QueryParams)
<T> PagedList<T> queryForList(MapConverter<T>, String, QueryParams, PageParams)
```

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






