## O-R Mapping 

### 概念

在 UDE-DB，要使用 OR Mapping，有兩種實作方式：

1. 底層選擇使用 Hibernate
2. 底層使用 JDBC，並
   * 實作各對應 Entity 類別的 EntityDAO。
   * 實作 DAOFinder 使可由 EntityClass 取得對應 DAOImpl instance (名稱轉換或查表)。
   
   看起來第二個選擇似乎比較麻煩，所以 UDE 提供工具元件 iisi-ude-tools-code-generator-db，
   可以由既有 DB 反向生成所需的 EntityClass 及 EntityDAOClass。
   
   
### 常見的 EntityClass 字尾

UDE 沒有規範 EntityClass 字尾，視專案及團隊的一致慣例即可。常見的選擇如下：

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

必要時，可複寫 DBFacadeJDBCConfiguration 中，有定義以下兩個 Bean。

``` java
@Bean
public DAOFinder jdbcDAOFinder() {
    return new JDBCDAOFinder();
}
```


