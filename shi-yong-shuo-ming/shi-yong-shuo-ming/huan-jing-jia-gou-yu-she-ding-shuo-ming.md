## 設定說明

### 引用元件

  * UDE使用MAVEN管理相依函式庫，開發環境需於MAVEN設定中加入公司內部 Repository 資訊，以取得UDE函式庫。

  ``` xml
 
    <repository>
        <id>IISIUDE-PUBLIC</id>
        <name>IISIUDE-PUBLIC</name>
        <url>http://192.168.57.11:8080/ude_maven</url>
        <releases><enabled>true</enabled></releases>
        <snapshots><enabled>true</enabled><updatePolicy>interval:5</updatePolicy></snapshots>
    </repository>
				
  ```

* **Maven** 設定
  
  pom.xml 加入以下項目。
  ``` xml
  <dependency>
      <groupId>com.iisigroup</groupId><artifactId>iisi-ude-db-core</artifactId>
  </dependency>
  <!-- 若用 hibernate 做為底層，就再加上(或改用) iisi-ude-db-core-hibernate -->
  <dependency>
      <groupId>com.iisigroup</groupId><artifactId>iisi-ude-db-core-hibernate</artifactId>
  </dependency>  
  ```

### 環境架構與設定說明

#### Hibernate

先以 Hibernate 為例，Spring Context 應在宣告 SessionFactory 處，加入 DBFacadeHibernateConfiguration，以建立資料庫模組的必要元件 Bean，主要是：

* PersistenceContext
* DBFacadeFactory

``` xml
<bean class="com.iisigroup.ude.configuration.DBFacadeHibernateConfiguration">
    <constructor-arg ref="mySessionFactory" />
</bean>
<bean id="mySessionFactory" class="org.springframework.orm.hibernate4.LocalSessionFactoryBean">
    <!-- SPRING-ORM -->
    <!-- 以下為測試範例內容，使用時應自行更改 -->    
    <property name="dataSource" ref="testDataSource1" />
    <property name="packagesToScan" value="sample.ude.db.entity" />
    <property name="hibernateProperties">
        <props>
            <prop key="hibernate.dialect">org.hibernate.dialect.HSQLDialect</prop>
        </props>
    </property>
</bean>
```


