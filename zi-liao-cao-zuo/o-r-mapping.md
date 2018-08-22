## O-R Mapping 

### 概念

在 UDE-DB，要使用 OR Mapping，有兩種實作方式可選：

1. 底層選擇使用 Hibernate
2. 底層使用 JDBC，並
   * 實作各對應 Entity 類別的 EntityDAO。
   * 實作 DAOFinder 使可由 EntityClass 取得對應 DAOImpl instance (名稱轉換或查表)。
   
   看起來第二個選擇似乎比較麻煩，所以 UDE 提供工具元件 iisi-ude-tools-code-generator-db，
   可以由既有 DB 反向生成所需的 EntityClass 及 EntityDAOClass。
   
   
實際的操作說明，請看後續「資料查詢」、「資料異動」章節。
   
### 常見的 EntityClass 字尾

UDE 沒有規範 EntityClass 字尾，視專案及團隊的一致慣例即可。
常見的選擇如下，選用原則也通常與專案的資料表命名慣例有關。

* **Entity**
* PO
* Type

### com.iisigroup.ude.db.dbaccess.dao.EntityDAO

定義 UDE ORMAPPING 的基本操作 : 

``` java
String           getTableName()
MapConverter<T>  getConverter()
boolean          checkEntity(PersistenceContext, T)
boolean          delete(PersistenceContext, T)
InsertResult     insert(PersistenceContext, T)
boolean          save(PersistenceContext, T, boolean)
T                selectByPK(PersistenceContext, T)
int              update(PersistenceContext, T)
```


### EntityDAOFactory

#### 使用 Hibernate

這個情境比較單純，內建的 HibernateDAOFactory 回傳各自的 new GenericDAOImpl<T>(entityClass) 實例。

#### 使用 JDBC

內建的 JDBCDAOFactory 負責基本的快取與建表查找。
實際由 EntityClass 查找 EntityDAO 則由 JDBCDAOFinder 實作。

必要時，可複寫 DBFacadeJDBCConfiguration 的以下定義。

``` java
@Bean
public DAOFinder jdbcDAOFinder() {
    return new JDBCDAOFinder();
}
```

* ##### JDBCDAOFinder

  預設的對應模式是  
  * DaoClass Name : 把 EntityClass 的字尾去掉，再加上 DAOImpl。
  * Dao package : 把 EntityClass 所在的 package 中的 entity 或 domain 代換為 dao，並在最末加上 .impl。
  
  可以用設定以下屬性以變動對應原則。
  ``` java
    protected String entityPackagePattern = "entity|domain";
    protected String entityClassSuffix = "Entity";
    protected String daoSuffix = "DAOImpl";
    protected boolean daoInImplPackage = true;
  ```
  








