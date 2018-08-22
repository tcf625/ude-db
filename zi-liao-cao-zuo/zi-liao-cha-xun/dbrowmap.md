# 資料操作

以下說明如何以 UDE-DB 元件進行資料庫的增、刪、改、查操作。
首先說明幾個常見的類別。

## 查詢結果：DBRowMap 

* 執行查詢時，每一筆資料列的回傳結果以 DBRowMap 表示。
* 有點像 JDBCTemplate 中，Map<String, Object> queryForMap(String sql) 回傳的 MAP，但功能較多
* 底層為 LinkedHashMap，依 resultSet 中的欄位順序排列。
* 操作：
  * 取得資料文字格式用 get(Object)
  * 取得原始資料用 getRawData
  * 欄位文字合併輸出：joinText(String separator, List<String> keys)
* lambda :

  ``` java
  static Function<DBRowMap, String> joinFunction(String separator, List<String> keys)
  ```
  
## 查詢結果集合：PagedList / PageInfo

PagedList 就是含分頁資訊的 List介面，UDE-DB 使用 PagedList 回應查詢結果。

``` java
public interface PagedList<E> extends List<E>, Serializable {
    PageInfo getPageInfo();
}
public class PageInfo implements Serializable {
    /** 目前頁次. */
    private int pageNo = 1;
    /** 每頁資料筆數. */
    private int pageSize = 0;
    /** 緦頁次. */
    private int pageCount = 0;
    /** 總資料筆數. */
    private long dataCount = 0;
    /** 索引值偏移量. */
    private int indexOffset = 1;
}
```

* ### PagedQueryResults 

  PagedQueryResults 繼承 PagedArrayList < DBRowMap \>，是多數查詢方法的預設回傳型態。
  
## 查詢參數：QueryParam & QueryParams




## 分頁查詢參數：PageParams






