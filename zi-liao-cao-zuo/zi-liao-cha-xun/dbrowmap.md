##  DBRowMap 

* 每一筆資料列的回傳結果以 DBRowMap 表示。
* 有點像 JDBCTemplate 中，Map<String, Object> queryForMap(String sql) 回傳的 MAP，但功能較多


* 操作：
  * 取得資料文字格式用  get(Object)
  * 取得原始資料用 getRawData
  * 欄位合併輸出：joinText(final String separator, final List<String> keys)