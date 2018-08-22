## 設定說明

### 引用元件

  UDE使用MAVEN管理相依函式庫，開發環境需於MAVEN設定中加入公司內部 Repository 資訊，以取得UDE函式庫。

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
  <!-- 若用 hibernate 做為底層，就再加上(或改用) -->
  <dependency>
      <groupId>com.iisigroup</groupId><artifactId>iisi-ude-db-core-hibernate</artifactId>
  </dependency>  
  ```

