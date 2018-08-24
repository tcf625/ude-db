### DBFacade的異動操作函式

```
### Native SQL

* 回傳異動筆數。


``` java
int execute(String)
int execute(String, QueryParams)
```

### OR-Mapping

* 

``` java
boolean delete(Object)
int update(Object)
InsertResult insert(Object)
void insertOrUpdate(Object)
```