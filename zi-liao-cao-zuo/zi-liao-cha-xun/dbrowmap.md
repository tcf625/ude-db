# 資料操作

以下說明如何以 UDE-DB 元件進行資料庫的增、刪、改、查操作。

##  DBRowMap 

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