## DBFacade 的查詢函式

### 基本多列查詢，取回原始資料

就是一般傳回 Select 語句，取得所有回傳結果的函式。
相關函式定義於 **SqlSelector** 介面，也就是說，這部分與使用低階 API 的效果基本上是相同的。

``` java
PagedQueryResults queryForList(String query)
PagedQueryResults queryForList(String query, QueryParams)
PagedQueryResults queryForList(String query, QueryParams, PageParams)
```

QueryParams、PageParams 參數與 PagedQueryResults 回傳型別定義如前述，query 查詢語句原則上是採用NativeSQL，除非：

    * 使用 Hibernate 實作
    * 且設定 dbFacade.setNativeQuery(false)
    * 這時查詢語句為 HQL

### 多列查詢 並 自行定義資料轉換器

如果每一列的原始資料，可以對應到某個 DTO 類別內容，我們也可以自行實作 MapConverter 進行轉換。

``` java
<T> PagedList<T> queryForList(MapConverter<T>, String)
<T> PagedList<T> queryForList(MapConverter<T>, String, QueryParams)
<T> PagedList<T> queryForList(MapConverter<T>, String, QueryParams, PageParams)
public interface MapConverter<T> {
    T convert(DBRowMap map); // 可以參考自動 CODE GENERATE 出來的 DAOImpl 實作
}
```

### 多列查詢 與 OR Mapping

* #### 以 '查詢字串'、'查詢參數' 取得Query結果

  * query 參數必須是完整 SQL 語句。
  * 試著以 Entity Class 對應的 DAO 的 MapConverter，將每列結果轉換為 T。
``` java
<T> PagedList<T> queryForList(Class<T>, String query)
<T> PagedList<T> queryForList(Class<T>, String query, QueryParams)
<T> PagedList<T> queryForList(Class<T>, String query, QueryParams, PageParams)
```

* #### 以 '物件類型' 做 OR-Mapping 查詢

  * 由 Entity Class 類別 決定查詢的 TABLE 對象。
  * 若有查詢條件，就使用 WhereBuilder 帶入。
``` java
<T> PagedList<T> queryForList(Class<T> cls) 
<T> PagedList<T> queryForList(Class<T> cls, PageParams pageParams) 
<T> PagedList<T> queryForList(Class<T> cls, StringWhereBuilder whereBuilder) 
<T> PagedList<T> queryForList(Class<T> cls, StringWhereBuilder whereBuilder, PageParams pageParams);
```

* ##### WhereBuilder





