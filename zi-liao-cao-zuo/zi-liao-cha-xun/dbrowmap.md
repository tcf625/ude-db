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

 為防止 SQL injection 攻擊，一般會建議使用 PreparedStatement 查詢。
 其中查詢語句的每一個 ? 參數，它的對應值就是一個 QueryParam 物件。
 使用時可以利用以下兩個建構子建立，也可以使用 QueryParams 提供的其它建構方法。
 
``` java
    public QueryParam(SqlType sqlType, Object value) {
        this.sqlType = sqlType;
        this.value = value;
    }
    public QueryParam(String value) {
        this.sqlType = SqlType.CHAR;
        this.value = value;
    }
```

* ### QueryParams   
 
  * QueryParams 實作 List < QueryParam \> 介面，是 QueryParam 的集合物件。
  * 在 ude-db 的多數函式，必要時，會以此類別做為傳入參數。
  * 建立後，可使用它提供的函式，加入 QueryParam 物件。
  
``` java
/**
 * SQL TYPE 依傳入 Object 決定
 * SqlType.NULL : value == null
 * SqlType.CHAR : java.lang.String
 * SqlType.NUMERIC : java.lang.Number
 * SqlType.JAVA_OBJECT : 其它
 */
void addParam(Object)
void addParam(SqlType, Object)
void addStringParam(String) // SQL Type 為 CHAR
void addRepeat(QueryParam, int times)   // 重複加入
void addRepeatString(String, int times) // 重複加入

```
* #### QueryParams.EMPTY 

  用於查詢沒有參數時的靜態常數。

* #### 靜態建構式
 
  為簡化使用，QueryParams 提供靜態建構式如下：
 
``` java
  static QueryParams from(Object...) // SQL Type 同 addParam(Object value) 原則
  static QueryParams fromString(List<String>) // SQL Type 為 CHAR
  static QueryParams fromString(String...)    // SQL Type 為 CHAR
```


## 分頁查詢參數：PageParams

進行分頁查詢時，也就是以每頁 N 筆，要求取得第 M 頁的資料列表時，所使用的參數類別。

``` java
1
```

* ### 靜態常數

  * PageParams.FIRST_ROW : 只取第一筆資料
  * PageParams.LIST_ALL : 取回所有資料





