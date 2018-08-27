
## Entity, DAO 程式碼產生器


* 需自行建立一個專案，dependence on iisi-ude-tools-code-generator-db
* 目前沒有把它 GUI 化的計畫，因為通常使用者限定於專案中的少數成員。
* 應該是工具程式，不是發行的專案內容。

### 啟動類別

``` java
public class JDBCGenerator {
  static {
    DBGeneratorConfigs.OutputPath.set("../");
    DBGeneratorConfigs.Default_ProjectPath.set("iut-core/");
    DBGeneratorConfigs.Default_DaoPackage.set("com.iisigroup.iut.base.dao.main.impl");
    DBGeneratorConfigs.Default_DomainPackage.set("com.iisigroup.iut.base.entity.main");
    DBGeneratorConfigs.Location_DatatypeMapping.set("classpath:config/datatype-mapping.properties");
    DBGeneratorConfigs.Location_DatatypeDefaultValue.set("classpath:config/datatype-default-value.properties");
    DBGeneratorConfigs.DomainClassSuffix.set("PO");
    DBGeneratorConfigs.DaoImplClassSuffix.set("DAOImpl");
  }
  public static void main(final String[] args) throws Exception {
    final GenerateJDBC generateJDBC = new GenerateJDBC();
    generateJDBC.setGlobalPattern("(?i)" + "(CITIES.*)");
    generateJDBC.run("classpath:config/generator-context-jdbc.xml");
  }
}    

```

* 在靜態建構子中，定義啟動參數
  * 設定檔路徑 
  * OutputPath : 預設檔案產出路徑。
  * Default_ProjectPath : 預設檔案所屬專案
  * 其它預設產出的所屬 package 及 class 名稱字尾。    
* main function
  * GlobalPattern : 
     * 符合格式的 TABLE/VIEW 名稱，才會被轉換。
     * 且有得以匹配的 TableConvertion。 
  * .run( spring_context_location ) : 開始執行程式碼產生。


### 設定檔

設定檔案建議置於 classpath 下即可，檔名即是啟動類別中指定的檔案名稱。

``` java
// config/datatype-default-value.properties
// config/datatype-mapping.properties
DBGeneratorConfigs.Location_DatatypeMapping.set("classpath:config/datatype-mapping.properties");
DBGeneratorConfigs.Location_DatatypeDefaultValue.set("classpath:config/datatype-mapping.properties");

// config/applicationContext-test-sample.xml 
generateJDBC.run("classpath:config/generator-context-jdbc.xml");
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
    <bean class="com.iisigroup.ude.tools.codegen.sample.config.SPEC_TABLE" />    
```

 * 資料庫連線設定 DataSource 可以有多個，工具程式會自動跑過每一組 DataSource
 
 * Converters 設定 : 為 JAVA CONFIG，內定 1~N 個  TableConvertion Bean

 
        
#### TableConvertion 

``` java
@Bean
public TableConvertion allTable() {
    final TableConvertion convertion = new AliasTableConvertion();
    convertion.setPermitRegexs("(?i)" + "(.*)");
    //convertion.setUnPermitRegexs("(?i)" + "(role.*)");
    
    // 此類型所屬的 package
    final String packageName = "com.iisigroup.iut.base.entity.main";
    convertion.setDomainPackage(packageName.toLowerCase());
    
    // 以 BaseDAOImplClassGenerator 生成 DAO 檔案 於專案 "iut-core/"
    convertion.addGeneratorFactory(BaseDAOImplClassGenerator.class, "iut-core/");
    
    // 以 IUTBasePOCG 生成 PO 檔案 於專案 "iut-core/"    
    convertion.addGeneratorFactory(IUTBasePOCG.class, "iut-core/");
    return convertion;
}
``` 

* 若自行實作 ClassGenerator ，可以有更多客制化定義。
 

 
