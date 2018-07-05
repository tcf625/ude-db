# DBFacade (查詢)

## 列表查詢


### * 以 '物件類型' 做 OR-Mapping 查詢

``` java
<T> PagedList<T> queryForList(Class<T> cls, StringWhereBuilder whereBuilder, PageParams pageParams);

    default <T> PagedList<T> queryForList(final Class<T> cls) {
        return this.queryForList(cls, PageParams.LIST_ALL);
    }

    default <T> PagedList<T> queryForList(final Class<T> cls, final PageParams pageParams) {
        return queryForList(cls, new WhereBuilder(), pageParams);
    }

    default <T> PagedList<T> queryForList(final Class<T> cls, final WhereBuilder whereBuilder) {
        return this.queryForList(cls, whereBuilder, PageParams.LIST_ALL);
    }
```


### * 以 '查詢字串'、'查詢參數' 取得Query結果