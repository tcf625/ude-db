## WhereBuilder ```( WhereClauses<String> )```

WhereBuilder 是目前 UDE-DB 提供的輔助類別中的一個重要結構。

* 主要作用：設定 SQL 語句中的 WHERE、OrderBy、GroupBy 子句。

* 在 2017 下半 ~ 2018 上半年度(v2.1.0~)，基於此類別之上，增加很多輔助類別與函式，但較為倉促，有些設計可能不夠完備。預計2018.12 的 v2.1.4 會把相關的單元測試及使用範例做較為完整的補充。


### 用途

WhereBuilder 的早期發展，是為了下列這種常見案例 : 
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
if (d==null){
  sql.append("and d is null ");
} else {
  sql.append("and d=? ");
  params.add(d);
}
return doQuery( sql.toString(), params);
```

程式碼看起來很長，而且可能會有一些後續維護上的成本。像是 SQL 敘述跟參數容易因人為失誤而不一致，若是測試不夠完備的話，很可能到上線後才出錯。

* 使用 WhereBuilder 的情境如下，可以見到比較清爽的業務邏輯。

``` java
// SimpleQueryExecutor implements IWhereBuilder 
final SimpleQueryExecutor executor = SimpleQueryExecutor.fromTable(xxx);
executor.equalsClause("a", a);
executor.equalsClause("b", b);
executor.betweenClause("c", c1, c2);
executor.required().equalsClause("d", d);
return executor.executeQuery(dbFacade);
```

* 2.1.0 ~ 2.1.3 支援串接語法，但 2.1.4 會移除，
  因為加入 .required(). 或其它後來加入的 and/or 子句特性時，容易造成誤解/誤用。

``` java
return SimpleQueryExecutor.fromTable(xxx) //
      .equalsClause("a", a) //
      .equalsClause("b", b) //
      .betweenClause("c", c1, c2) //
      .required().equalsClause("d", d) //
      .executeQuery(dbFacade); 
```



### 介面說明 WhereClauses < T \>

泛型參數 T 是為了之後會介紹的另一個實作分支 EntityWhereBuilderWrapper 而設，表示如何傳入 COLUMN 定義
。在一般用法下，T 應該是 String 型別，直接傳入 Column 名稱。

#### 比對條件子句

* 預設逐一加入的子句，在彼此之間，會使用 AND 連接。
* 基本原則是如果傳入值為空白(isBlank，包含由空白字元組成的情況)，就不會加入對應子句。

``` java
betweenClause(String, T, SqlType, Object, Object)
betweenClause(T, String, String)

clause(String, T, OP, SqlType, Object)
clause(T, OP, SqlType, Object)
clause(T, OP, String)

equalsClause(String, T, SqlType, Object)
equalsClause(T, SqlType, Object)
equalsClause(T, String)

inClause(String, T, List<String>)
inClause(String, T, QueryParams)
inClause(T, List<String>)
inClause(T, String[])

likeClause(String, T, String)
likeClause(T, String)

notInClause(String, T, QueryParams)
notNullClause(String, T)

nullClause(String, T)
nullClause(T)

startWithClause(String, T, String)
startWithClause(T, String)
```


####

``` java
specClause(String, QueryParam...)
specClause(String, QueryParams)
```

1111



``` java
addAndClauses(Consumer<IWhereBuilder<T>>)
addNotClauses(Consumer<IWhereBuilder<T>>)
addOrClauses(Consumer<IWhereBuilder<T>>)

andClauses()
notClauses()
orClauses()
required()
```


1111



``` java
extraClause(ExtraClauseOP, String)
```




 






