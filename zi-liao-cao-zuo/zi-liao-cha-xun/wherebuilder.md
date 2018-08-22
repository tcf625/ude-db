## WhereBuilder

WhereBuilder 是目前 UDE-DB 提供的輔助類別中的一個重要結構。

* 用於設定 SQL 語句中的 WHERE 條件子句。
* 類別階層命名上，有點混亂：
  * 因為是先有最下層的實作 WhereBuilder 再逐一向上抽出介面及分支實作。
  * 如果 UDE 再有大改版，或許會考慮重構命名。
* 2017 下半 ~ 2018 上半年度(v2.1.0~)，基於此基礎上增加很多輔助類別與函式，如之後會提到的 SimpleQueryExecutor 等等，預計2018.12 的 v2.1.4 會把相關的單元測試及使用範例做較為完整的補充。


### 用途

WhereBuilder 的早期發展，是為了一種常見案例 : 

這是在一些早期專案中，可能會看到的寫法

``` java
StringBuilder sql = new StringBuilder("select * from xxx where 1=1 ");
List<String> params = new ArrayList<>();


if (StringUtils.isNotBlank(a)) {
  sql.append("and a=? ");
  params.add(a);
}
if (StringUtils.isNotBlank(b)) {
  sql.append("and b=? ");
  params.add(b);
}
if (StringUtils.isNotBlank(c1) && StringUtils.isNotBlank(c2)) {
  sql.append("and (c between ? and ?) ");
  params.add(c1);
  params.add(c2);  
}
sql.append("and d=? ");
params.add(d);
return doQuery( sql.toString(), params);
```

程式碼看起來很長，而且可能會有一些後續維護上的成本。
SQL 敘述跟參數容易因人為失誤而有不一致，若是測試不夠完備的話，很可能到上線後才出錯。




 






