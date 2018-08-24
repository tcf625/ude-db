### EntityUpdater & EntityDeleter

#### 簡介

 * 操作單一 TABLE
 * 加入 WHERE 條件式時，以 CODE-GEN 產出的 ColumnDefine Enum 為參數
   避免因開發期間, 因為資料庫 SCHEMA 的頻繁修改, 但使用字串設定時的不一致。
   
   
-----
   
* 為什麼沒有 Insert 的對應類別  : 
  * 用 DBFacade.insert 就好了。