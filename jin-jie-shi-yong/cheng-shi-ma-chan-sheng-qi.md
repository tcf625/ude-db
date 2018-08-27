
## Entity, DAO 程式碼產生器


* 需自行建立一個專案，dependence on iisi-ude-tools-code-generator-db
* 目前沒有把它 GUI 化的計畫，因為通常使用者限定於專案中的少數成員。
* 應該是工具程式，不是發行的專案內容。

### 設定檔

設定檔案建議置於 classpath 下即可，檔名原則上可以需求調整。

```
config/applicationContext-test-sample.xml 
config/datatype-default-value.properties
config/datatype-mapping.properties
```

#### Spring : applicationContext-test-sample.xml

``` xml
    <context:annotation-config />

    <bean class="com.iisigroup.ude.configuration.UdeBasicConfiguration" />
    <bean id="MainDataSource" class="org.apache.commons.dbcp.BasicDataSource" destroy-method="close">
        <property name="driverClassName" value="com.mysql.cj.jdbc.Driver" />
        <property name="username" value="root" />
        <property name="url" value="jdbc:mysql://192.168.57.11:13306/iUTDB_SpringMVC?characterEncoding=UTF-8&amp;useSSL=false" />
        <property name="password" value="passw0rd" />
    </bean>
    <!-- Converters -->
    <bean class="com.iisigroup.ude.tools.codegen.sample.config.ALL_TABLES" />
```

 * 資料庫連線設定 DataSource 可以有多個，工具程式會自動跑過每一組 DataSource
   * 唯一的限