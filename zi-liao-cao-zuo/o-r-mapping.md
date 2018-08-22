## O-R Mapping 

### 概念

在 UDE-DB，要使用 OR Mapping，有兩種實作方式：

1. 底層選擇使用 Hibernate
2. 底層使用 JDBC，並
   * 實作各對應類別的 DAO。
   * 實作 DAOFinder 使可由 EntityClass 取得對應 DAOImpl instance (名稱轉換或查表)。
   
   看起來第二個選擇似乎比較麻煩，所以 UDE 提供工具元件 iisi-ude-tools-code-generator-db，
   可以由既有 DB 反向生成所需的 EntityClass 及 DAOClass。
   
   
### 常見的 EntityClass 字尾

UDE 沒有規範 EntityClass 字尾，視專案及團隊的一致慣例即可。常見的選擇如下：

* **Entity**
* PO
* Type


### DAO Finder

DBFacadeJDBCConfiguration 中，有定義以下兩個 Bean
``` java
@Bean
public JDBCDAOFactory jdbcDAOFactory() {
    final JDBCDAOFactory jdbcDAOFactory = new JDBCDAOFactory();
    jdbcDAOFactory.setDaoFinder(jdbcDAOFinder());
    return jdbcDAOFactory;
}
@Bean
public DAOFinder jdbcDAOFinder() {
    return new JDBCDAOFinder();
}
```
