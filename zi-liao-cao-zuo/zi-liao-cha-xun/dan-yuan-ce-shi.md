### 單元測試 : 


#### 驗證 ~SqlExecutor 產出

產出 SQL 應如同預期，尤其是有動態條件變動的時候。

可利用 SQLAssert.assertEquals 比對測試，以忽略大小寫、空格、換行等差異。
或 ParameterizedSQLAssert 同時比對參數數量與內容。

``` java
@Test
public void test() {
    final UpdateSqlExecutor executor = new UpdateSqlExecutor();
    executor.setByClause("Col_1", "Col_0+1");
    executor.equalsClause("id", "I00001");
    assertSQL(executor.toParameterizedSQL("table2"), 
              "update table2 set col_1=col_0+1 where id=?", 1);
              
    assertSQL(executor.toParameterizedSQL("table2"), "" 
            + "update table2        " 
            + "   set col_1=col_0+1 " 
            + " where id=?          ", 1);
}

private void assertSQL(final ParameterizedSQL parameterizedSQL, final String expectedSQL, final int size) {
    SQLAssert.assertEquals(expectedSQL, parameterizedSQL.sql);
    assertEquals(size, parameterizedSQL.queryParams.size());
}
```