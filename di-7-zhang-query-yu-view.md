# 第7章 Query 與 View

**Query**

* Query的概念
* Query的建立
* Query的使用

**View**

* View 的概念
* View 的建立
* View 的使用
* View 與資料更新





**創造 View**

*創建檢視
*使用檢視
*在檢視中更新數據
*綜合檢視
*使用離線數據
*優化檢視性能

**緩衝模式**
```text
CLOSE TABLES

SET DEFAULT TO c:\vfp\Lesson1
SET MULTILOCKS ON && 這是一定要的

SELECT 0
USE product SHARED
CURSORSETPROP("Buffering",5,"product") && 資料表緩衝模式
=MESSAGEBOX("緩衝資料表")
BROWSE

LOCAL m.nTest

m.nTest = 2

DO CASE
CASE nTest = 1 
  =MESSAGEBOX("寫入真實資料表")
  =TABLEUPDATE(.T.,.T.,"Product") && 寫入資料表
CASE nTest = 2
  =MESSAGEBOX("還玩資料表")
  =TABLEREVERT(.T.,"Product") && 還原資料表
ENDCASE
BROWSE 
USE

* 有沒有真的寫入
=MESSAGEBOX("真實資料表")
SELECT 0
USE product SHARED
BROWSE
USE

Close Tables && 關閉所有資料表
RETURN 
```text


**創建 Local View**
```text
CREATE SQL VIEW 
CREATE SQL VIEW product_view AS SELECT * FROM testdata!products 
```text

**使用存儲的 SQL SELECT 語句創建視圖**
```text
emp_cust_sql = "SELECT ;
  employee.emp_id, ;    
  customer.cust_id, customer.emp_id, ;    
  customer.contact, customer.company ;    
    FROM employee, customer ;    
       WHERE employee.emp_id = customer.emp_id" 

CREATE SQL VIEW emp_cust_view AS &emp_cust_sql 

* 提示   在視圖設計器中，您可以打開現有視圖，然後復制只讀 SQL 字符串並將其粘貼到您的代碼中，作為以編程方式創建視圖的快捷方式。
```text


**修改視圖**
```text
OPEN DATABASE testdata 

MODIFY VIEW product_view  
```text

**重命名視圖**
```text
RENAME VIEW product_view TO products_all_view 
```text

**刪除視圖**
```text
DELETE VIEW product_view 
DROP VIEW customer_view 
```text

**創建多表視圖**
```text
* 使用 WHERE
OPEN DATABASE testdata CREATE SQL VIEW cust_orders_emp_view AS ;    
  SELECT * FROM testdata!customer, ;       
  testdata!orders, testdata!employee ;    
  WHERE customer.cust_id = orders.cust_id ;     
  AND orders.emp_id = employee.emp_id 
```text

```text
* 使用 Join 
OPEN DATABASE testdata CREATE SQL VIEW cust_orders_view AS ;    
  SELECT * FROM testdata!customer ;       
  INNER JOIN testdata!orders ;       
  ON customer.cust_id = orders.cust_id 
```text
```text
OPEN DATABASE testdata CREATE SQL VIEW cust_orders_view AS ;    
  SELECT * FROM testdata!customer ;       
  LEFT OUTER JOIN testdata!orders ;       
  ON customer.cust_id = orders.cust_id 
```text
```text
OPEN DATABASE testdata CREATE SQL VIEW cust_orders_emp_view AS ;    
  SELECT * FROM testdata!customer ;       
  INNER JOIN testdata!orders ;       
  ON customer.cust_id = orders.cust_id ;       
  INNER JOIN testdata!employee ;       
  ON orders.emp_id = employee.emp_id 
```text



**創建命名連接**
```text
OPEN DATABASE testdata 
CREATE CONNECTION remote_01 DATASOURCE sqlremote userid password 
```text

**確定現有連接**
```text
OPEN DATABASE testdata 
DISPLAY CONNECTIONS 
```text

**創建遠程視圖**
```text
OPEN DATABASE testdata  
CREATE SQL VIEW product_remote_view REMOTE;     
  CONNECTION remote_01 ;  &&  CONNECTION MyDSN
  AS SELECT * FROM products 



*如果在遠程視圖設計器中連接兩個或多個表，設計器使用內部連接（或等連接）並將連接條件放在 WHERE 子句中。
*如果要使用外連接，遠程視圖設計器僅提供左外連接，即 ODBC 支持的語法。如果您需要右外連接或全外連接，或者只想對左外連接使用本機語法，請以編程方式創建視圖。
```text

**使用視圖**
```text
OPEN DATABASE testdata 
USE product_view  
BROWSE 

*如果視圖基於本地表，Visual FoxPro 還會在單獨的工作區中打開基表。
```text

**限制視圖的範圍**
```text
SELECT * FROM customer ;    WHERE customer.country = 'Sweden' 
```text

**創建參數化視圖**
```text
*使用帶有 ? 的 CREATE SQL VIEW 

OPEN DATABASE testdata 
CREATE SQL VIEW customer_remote_view ;    
CONNECTION remote_01 ;    
AS SELECT * FROM customer ;    
WHERE customer.country = ?cCountry 

cCountry = 'Sweden' 
USE Testdata!customer_remote_view IN 0 
BROWSE 
```text


**提示用戶輸入參數值**
```text
OPEN DATABASE testdata 
CREATE SQL VIEW customer_remote_view ;    
CONNECTION remote_01 ;    
AS SELECT * FROM customer ;    
WHERE customer.cust_id = ?'my customer id' 
USE customer_remote_view 


**在多個工作區中打開視圖**
```text
OPEN DATABASE testdata 
CREATE SQL VIEW product_remote_view ;    
  CONNECTION remote_01 ;    
  AS SELECT * FROM products 
USE product_remote_view 
BROWSE 
SELECT 0 
USE product_remote_view NOREQUERY 
BROWSE 
*會在同一結果集上再次打開游標
```text

**使用 AGAIN 開兩個**
```text
OPEN DATABASE testdata 
USE product_remote_view 
BROWSE 
USE product_remote_view AGAIN in 0 
BROWSE 
```text


**顯示視圖的結構**
```text
OPEN DATABASE testdata 
USE customer_remote_view NODATA in 0 
BROWSE 

*提示   使用 NODATA 子句比在視圖或游標上使用 MaxRecords 屬性設置 0 更有效。
```text


**在視圖上**
```text
*您可以使用 INDEX ON 
*您可以使用 SET RELATION 
*您可以使用 DBSETPROP( ) 函數修改遠程視圖字段的 DataType 屬性設置
*使用視圖時自動打開的本地基表在關閉視圖時不會自動關閉
*默認情況下，視圖使用樂觀行緩衝進行緩衝。您可以將其更改為表緩衝
```text

**視圖可更新**
```text
DBSETPROP('cust_view','View','Tables','customer') 
DBSETPROP('cust_view.cust_id','Field','KeyField',.T.) 
DBSETPROP('cust_view.cust_id','Field','UpdateName',; 'customer.cust_id') 
DBSETPROP('cust_view.cust_id','Field','Updatable',; .T.) 
DBSETPROP('cust_view','View','SendUpdates',.T.) 
```text

**更新視圖中的多個表**
```text
*創建一個訪問兩個表中字段的視圖。
CREATE SQL VIEW emp_cust_view AS ;    
  SELECT employee.emp_id, ;    
  employee.phone, customer.cust_id, ;    
  customer.emp_id, customer.contact, ;    
  customer.company ;    
  FROM employee, customer ;    
  WHERE employee.emp_id = customer.emp_id

*設置要更新的表。
DBSETPROP('emp_cust_view', 'View', 'Tables', 'employee, customer')
*設置更新名稱。
DBSETPROP('emp_cust_view.emp_id', 'Field',  'UpdateName', 'employee.emp_id' ) 
DBSETPROP('emp_cust_view.phone', 'Field',   'UpdateName', 'employee.phone'  ) 
DBSETPROP('emp_cust_view.cust_id', 'Field', 'UpdateName', 'customer.cust_id') 
DBSETPROP('emp_cust_view.emp_id1', 'Field', 'UpdateName', 'customer.emp_id' ) 
DBSETPROP('emp_cust_view.contact', 'Field', 'UpdateName', 'customer.contact') 
DBSETPROP('emp_cust_view.company', 'Field', 'UpdateName', 'customer.company')


*為 Employee 表設置單字段唯一鍵。
DBSETPROP('emp_cust_view.emp_id', 'Field', 'KeyField', .T.)
*為 Customer 表設置一個兩字段的唯一鍵。
DBSETPROP('emp_cust_view.cust_id', 'Field', 'KeyField', .T.)
DBSETPROP('emp_cust_view.emp_id1', 'Field', 'KeyField', .T.)
 
*設置可更新字段。通常，關鍵字段不可更新。
DBSETPROP('emp_cust_view.phone',   'Field'  , 'UpdatableField', .T.)
DBSETPROP('emp_cust_view.contact', 'Field'  , 'UpdatableField', .T.) 
DBSETPROP('emp_cust_view.company', 'Field'  , 'UpdatableField', .T.)
 
*激活更新功能。
DBSETPROP('emp_cust_view', 'View', ;             'SendUpdates', .T.)

*修改視圖中的數據。
GO TOP
REPLACE employee.phone WITH "(206)111-2222" 
REPLACE customer.contact WITH "John Doe"
 
*通過更新 Employee 和 Customer 基表來提交更改。
TABLEUPDATE()
```text

**預設值**
```text
OPEN DATABASE testdata 
USE VIEW customer_view ?
DBSETPROP ('Customer_view.maxordamt', 'Field', 'DefaultValue', 1000) 
```text

**創建規則**
```text
OPEN DATABASE testdata 
USE VIEW orditems_view 
DBSETPROP('Orditems_view.quantity','Field', 'RuleExpression', 'quantity >= 1') 
DBSETPROP('Orditems_view.quantity','Field', 'RuleText'      , '"Quantities must be greater than or equal to 1"') 
```text

**組合視圖**
```text
OPEN DATABASE testdata  CREATE SQL VIEW remote_orders_view ;
    CONNECTION remote_01 ;
        AS SELECT * FROM orders CREATE SQL VIEW local_employee_remote_orders_view ;
            AS SELECT * FROM testdata!local_employee_view, ;
                testdata!remote_orders_view ;
                    WHERE local_employee_view.emp_id = ;
                           remote_orders_view.emp_id 
```text



**創建離線視圖**
```text
CREATE SQL VIEW showproducts ;
    CONNECTION dsource ;
        AS SELECT * FROM Products INNER JOIN Inventory ;
            ON Products.ProductID = Inventory.ProductID ;
CREATEOFFLINE("showproducts")
```text

**離線使用數據**
```text
USE Showproducts 
```text

**離線管理數據**
```text
USE Showproducts ADMIN 
```text

**使用離線視圖更新本地表**
```text
*重新連接主機並打開視圖
USE myofflineview ONLINE EXCLUSIVE
*檢查更新衝突並根據需要進行更新
BEGIN TRANSACTION 
IF TABLEUPDATE (2, .F., "myofflineview")    
    END TRANSACTION 
ELSE    
    MESSAGEBOX("Error Occurred: Update unsuccessful.")
    ROLLBACK 
ENDIF
```text

**設置連使用 TABLEUPDATE( ) 來處理您的事務**
```text
hConn1 = CURSORGETPROP("CONNECTHANDLE","myview") ; 
SQLSETPROP(hConn1,"TRANSACTIONS",2) 

*重新連接到主機並打開視圖。
USE myofflineview ONLINE EXCLUSIVE

*在視圖上設置連接以手動處理事務。
SQLSetProp(liviewhandle  ,"transactions",2) 
SQLSetProp(custviewhandle,"transactions",2) 
SQLSetProp(ordviewhandle ,"transactions",2) 
*處理更新和更新衝突。
IF NOT TABLEUPDATE(.T.,.F.,"lineitemsview")    
    =SQLROLLBACK(ordviewhandle)    
    =MESSAGEBOX("Can't update line items table")
    IF NOT TableUpdate(.T.,.F.,"ordersview")
        =SQLROLLBACK(liviewhandle)
        =MESSAGEBOX("unable to update the orders table")
        IF NOT TABLEUPDATE(.T.,.F.,"customerview")
            =SQLROLLBACK(custviewhandle)
            =MESSAGEBOX("Can't update customer table")
        Else *# check out failure scenarios
           IF NOT SQLCOMMIT(liviewhandle)
                =SQLROLLBACK(liviewhandle)
                IF NOT SQLCOMMIT(ordviewhandle)
                    =SQLROLLBACK(ordviewhandle)
                    IF NOT SQLCOMMIT(custviewhandle)                   
                        =SQLROLLBACK(custviewhandle)                
                    ENDIF             
                ENDIF          
            ENDIF       
        ENDIF    
    ENDIF 
ENDIF
```text

**更新一條記錄可以使用自動事務**
```text
USE customerview ONLINE EXCLUSIVE 
GO TO 3    
IF TABLEUPDATE (0, .F. , "customerview") && Current Row by Key Only
  * conflict handling code    
ENDIF 
```text

**取消離線更新**
```text
DROPOFFLINE("myview") 
```text

**設置 MaxRecords**
```text
OPEN DATABASE testdata 
USE VIEW remote_customer_view 
DBSETPROP ('Remote_customer_view', 'View','MaxRecords', 50) 
```text

**共享連接**
```text
CREATE SQL VIEW product_view_remote ;
    CONNECTION remote_01 SHARE AS ;
        SELECT * FROM products 
USE product_view_remote 
```text

**判斷連接是否繁忙**
```text
nConnectionHandle=CURSORGETPROP('ConnectHandle') 
SQLGETPROP(nConnectionHandle, "ConnectBusy") 
```text









**練習**

