## WhereBuilder ```( WhereClauses<String> )```

WhereBuilder 是目前 UDE-DB 提供的輔助類別中的一個重要結構。

* 主要作用：設定 SQL 語句中的 WHERE、OrderBy、GroupBy 子句。

* 在 2017 下半 ~ 2018 上半年度(v2.1.0~)，基於此類別之上，增加很多輔助類別與函式，但較為倉促，有些設計可能不夠完備。預計2018.12 的 v2.1.4 會把相關的單元測試及使用範例做較為完整的補充。


### 用途

早期發展 WhereBuilder 這個類別，是為了下列常見案例，這是在一些專案中，可能會看到的寫法 : 

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

// QuerySqlExecutor implements WhereClauses<String>

final QuerySqlExecutor executor = QuerySqlExecutor .fromTable(xxx);
executor.equalsClause("a", a);
executor.equalsClause("b", b);
executor.betweenClause("c", c1, c2);
executor.required().equalsClause("d", d);
return executor.executeQuery(dbFacade);
```

* 2.1.0 ~ 2.1.3 支援串接語法，但 2.1.4 會移除，
  因為加入 .required(). 或其它後來加入的 and/or 子句特性時，容易造成誤解/誤用。

``` java
return QuerySqlExecutor.fromTable(xxx) //
      .equalsClause("a", a) //
      .equalsClause("b", b) //
      .betweenClause("c", c1, c2) //
      .required().equalsClause("d", d) //
      .executeQuery(dbFacade); 
```



### 介面說明 WhereClauses < T \>

泛型參數 T 是為了之後會介紹的另一個實作分支 EntityWhereBuilderWrapper 而設，表示如何傳入 COLUMN 定義
。在一般用法下，T 應該是 String 型別，直接傳入 Column 名稱。

* 預設逐一加入的子句，彼此之間會使用 AND 連接。

#### 比對條件子句

* 基本原則是如果傳入值為 NULL 或空白(isEmpty)，就不會加入對應子句。

* 參數設計原則如下
  * String tableAlias : 表格別名，若有產出的欄位名稱就會是
  * T column : 欄位名稱，若有表格別名，產出的欄位名稱就會是 {tableAlias}.{column}
  * 比對值，採用以下兩種之一：
     * String value : 大多數情況適用，SqlType 定義為 CHAR。
     * SqlType type / Object value：指定 SqlType 。

* ##### equalsClause (...)

典型的比對條件，產出子句為：{column}=?

* ##### clause (...)

與 equalsClause 相比，多加上一個 OP 參數欄位，產出子句為：``` {column} {OP} ? ```
OP 種類定義如下：
``` java
public enum OP {
   LT("<"), LE("<="), GT(">"), GE(">="), EQ("="), NE("<>");
   ... 
}
```

* ##### betweenClause (...)

  參數的比對值會有兩個，value1/value2，使用時應該要自行確認輸入順序。
  當兩者都不是空字串時，才會產出子句。
  產出的子句像這樣： ```(col >= ? and col <= ?)``` ；而非： ```(col between ? and ?)```  
  是因為我們 informix 的 DBA 建議採用第一種語法。雖然根據查到的一些資料、討論，兩者在多數 DB 上效能應該沒有出入才對。
  * https://stackoverflow.com/questions/2692593/between-operator-vs-and-is-there-a-performance-difference

* ##### inClause (...) & notInClause (...)

傳入比對值的型別是 List<String> / QueryParams。
只要傳入集合中有值，就產出子句。
如果傳入的數量只有一個，就產出 ``` {column}=? ``` 或 ``` {column} <> ? ``` 。
如果傳入的數量有多個，就是 IN 子句 ``` {column} in (?, ?, ...) ``` 或 ``` {column} not in (?, ?, ...) ```。
但應該要注意參數的數量，不要超過 SQL 的限制。

* ##### likeClause (...) & startWithClause (...)

  
  likeClause/startWithClause 只接受 String 型別輸入。
  產出子句如 :  
  
  * https://stackoverflow.com/questions/8247970/using-like-wildcard-in-prepared-statement


* ##### nullClause & notNullClause (...)

  因為沒有可比對值，所以產出就是 ``` {column} is null ``` 或 ``` {column} is not null ``` 。


#### 其它語法

其它未能支援的複雜 SQL 子句 ，可使用此函式直接加入。

``` java
specClause(String, QueryParam...)
specClause(String, QueryParams)
```


#### 串接 OR / AND / NOT 語法

使用 lambda 風格較佳，比較不會誤用 xxxClauses 回傳的子句 BUILDER .

``` java
void addAndClauses(Consumer<IWhereBuilder<T>>)
void addNotClauses(Consumer<IWhereBuilder<T>>)
void addOrClauses(Consumer<IWhereBuilder<T>>)

WhereClauses<T> andClauses()
WhereClauses<T> notClauses()
WhereClauses<T> orClauses()
```


#### 必要條件

''' java 
void addRequiredClauses(Consumer<IWhereBuilder<T>>)
WhereClauses<T>  required()
'''

required 回傳的 WhereClauses，有傳入值的子句方法，在輸入為空時，會有不同的產出。如 

* equals : 會變為 is null 或 =? 
* <> : 會變為 is not null 或 <>? 
* ```> < >= <=``` 等 OP，如傳入為 NULL，會丟出 DB4005E 例外

* inClause 
  * in null ==>  ```{column} is null  ```
  * in {}   ==>  ```1=0```
* like / startWith 
  * like null / like '%%' => 行為一樣，不加入任何子句。

#### GROUP BY / ORDER BY

``` java 
// extraClause(ExtraClauseOP, String)

qe.extraClause(ExtraClauseOP.OrderBy, "site");
qe.extraClause(ExtraClauseOP.OrderBy, "age desc");
qe.extraClause(ExtraClauseOP.OrderBy, "gender");
// Order by site, age desc, gender
```



------------

TODO : 再加上以下兩個，常用的大概就差不多了

void  inTable(subQuery) // WHERE column_name IN (SELECT STATEMENT); 
void  exists (subQuery) // EXISTS (SELECT column_name FROM table_name WHERE condition); 

 






