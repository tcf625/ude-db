# DBFacade (查詢)

## 列表查詢


### * 以 '物件類型' 做 OR-Mapping 查詢

``` java
<T> PagedList<T> queryForList(Class<T> cls) 
<T> PagedList<T> queryForList(Class<T> cls, PageParams pageParams) 
<T> PagedList<T> queryForList(Class<T> cls, WhereBuilder whereBuilder) 
<T> PagedList<T> queryForList(Class<T> cls, StringWhereBuilder whereBuilder, PageParams pageParams);
```


### * 以 '查詢字串'、'查詢參數' 取得Query結果