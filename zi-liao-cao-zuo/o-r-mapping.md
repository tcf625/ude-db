概念: 

實作方式有二
1. 底層選擇 Hibernate
2. 底層用 JDBC，並實作各類的 DAO，實作 DAOFinder 使可由 EntityClass 取得對應 DAOImpl
   * EntityClass & DAOClass 可使用 iisi-ude-tools-code-generator-db 提供元件，由既有 DB 反向生成。