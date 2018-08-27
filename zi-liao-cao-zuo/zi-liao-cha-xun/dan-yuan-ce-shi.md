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
    
    ParameterizedSQLAssert.assertSQL(executor.toParameterizedSQL(), 
              "update table2 set col_1=col_0+1 where id=?", "I00001");
              
    ParameterizedSQLAssert.assertSQL(executor.toParameterizedSQL(), "" 
            + "update table2        " 
            + "   set col_1=col_0+1 " 
            + " where id=?          ", "I00001");
}

```


#### NativeSQLAnswer 

另一個驗證模式為使用 NativeSQLAnswer 。
配合 MockUtils 產出的 DBFacade ，可以把每一次 SQL 呼叫記錄下來比對。

``` java
final DBFacade dbFacade = MockUtils.getMock(DBFacade.class);

@Test
public void test_updateTable_By_clause() {
    final NativeSQLAnswer answer = DBMockUtils.mockNativeQuery();
    final UpdateSqlExecutor executor = UpdateSqlExecutor.withTable("table1");
    executor.setByClause("Col_1", "Col_0+1");
    executor.equalsClause("id", "I00001");
    executor.updateTable(this.dbFacade);
    answer.assertSQL(0, "update table1 set col_1=col_0+1  where id = ? ", "I00001");
}
```

