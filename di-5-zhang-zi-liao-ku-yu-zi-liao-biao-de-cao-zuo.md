# 第5章 資料庫與資料表的操作

**資料庫系統**

* 資料與資料處理
* 計算機資料管理
* 資料庫系統的組成

**關連式資料庫**

* 概念模型
* 資料模型
* 關連式式模型
* 關連式式運算

**設計資料庫**

* 分析需求
* 確定所需的資料表
* 設計表的結構
* 確定表的主關鍵字
* 確定表之間的關連式

**建立資料庫與資料表**

* 建立資料庫
* 建立資料表
* 定義資料表結構
* 輸入資料記錄
* 修改資料表結構
* 設置資料字典信息
* 通過命令窗來新增、修改、刪除資料

**操作資料表**

* 打開和關閉表
* 顯示表的資料記錄
* 移動記錄指針
* 查找記錄
* 新增記錄
* 刪除記錄
* 修改記錄
* 過濾資料表
* 資料表的複製和匯入
* 記錄與陣列的資料交換
* 記錄的統計

**操作資料庫**

* 打開資料庫及設計器
* 關閉資料庫
* 向資料庫加入資料表
* 從資料庫移出資料表
* 自由資料表
* 刪除資料庫
* 資料庫的清理與檢驗

**索引**

* 索引的概念
* 索引的建立
* 索引的使用
* 索引的刪除
* 物理排序

**多表的使用**

* 工作區
* 使用其他工作區的表
* 資料表之間的臨時關聯

**聯繫及參照完整性**

* 聯繫
* 參照完整性
* 資料完整性




**建立資料庫 使用滑鼠**
```text
* 新增資料庫 Database
1.新增資庫：使用專案 Data 頁 -> New 
2.新增資料表： 滑鼠右鍵 -> New 
```
**建立資料庫 使用程式**
```text
* 注意：路徑問題 
CREATE DATABASE Data1 && 也可以使用專案視窗建立
* 建得將 Data1 加入專案中
```
**刪除資料庫 使用程式** 
```text
DELETE DATABASE Data1 DELETETABLES RECYCLE && 刪除資料庫 連同所有 Tables
*DELETE DATABASE Data1 && 刪除資料庫 但是不要刪 Tables
```
**開啟料庫** 
```text
Open DataBase Data1
Open DataBase Data2
```
**指定作用中的資料庫**
```text
Set DataBase To Data1
```
**關閉當前資料庫**
```text
Set DataBase To Data1
Close DataBase && 關閉Data1 與相關 Table
Set DataBase To Data2
Close DataBase && 關閉Data2 與相關 Table
```
**關閉所有資料庫**
```text
Close DataBase ALL 
```

**新增資料表**
```text
Open DataBase Data1
Open DataBase Data2
SET Database to Data1 && 指定作用中的資料庫
CREATE TABLE Product (Prno c(3),Prna c(10),Qty n(18)) && 加到 Data1  
```
**新增資料表**
```text
Open DataBase Data1
Open DataBase Data2
SET Database to Data1 && 指定作用中的資料庫
CREATE TABLE Product (Prno c(3),Prna c(10),Qty n(18)) && 加到 Data1  
```
**刪除資料表**
```text
REMOVE TABLE Product DELETE RECYCLE
*REMOVE TABLE Product && 只有移出資料庫 沒有刪除檔案
```
**建立 Free TABLE 使用滑鼠**
```text
1.專案 Data 頁 -> 指定 Free Table
2.點擊 New
```
**建立 Free TABLE 使用程式**
```text
Create Table Product Free (Prno c(3),Prna c(10),Qty n(18)) && 多了 Free
* 記得將 Product 加入專案中
```
**Table 的開啟**
```text
Select 1
Use Product 
Select 2
Use Customer

或

Select 1
Use Data1!Product 
Select 2
Use Data1!Customer
```
**建立 Cursor 一**
```text
Create Cursor AAA (No c(10), Na (20)) && 只存在記憶體
```
**建立 Cursor 二**
```text
Select 1
use Product 
Select * from Product into cursor Product2 ReadWrite

select Product
Browse
Use 

Select Product2
Browse
Use 
```
**Table 的別名**
```text
Select 1 
use product alias Prd
Select 2
use Customer alias Cus

select Prd
Browse
use 

select Cus
Browse
use 
```
**Table 的關閉**
```text
Select 1
Use
Select 2
Use

或

Select Product
Use
Select Customer
Use 

或
Use in Product
Use in Customer

或
Close Tables All 

或
Clear All && 連資料庫也關了
```
**資料的顯示 使用程式**
```text
use Product 
Browse
```
**資料的顯示 使用物件 Form 物件加入 Grid 物件**
```text
1.開啟 Product
2.Do Form Form1
3.關閉 Product
```
**加入資料 使用 Append Blank**
```text
Append Blank
Replace Prno with "001",Prna with "電腦",Qty with 100,
Append Blank
Replace Prno with "002",Prna with "鍵盤",Qty with 200,
Append Blank
Replace Prno with "003",Prna with "滑鼠",Qty with 300,
```
**加入資料 使用 Append From 其他DBF**
```text
select 1
use Product 
Append from c:\xxx\OtherProduct.dbf
```
**加入資料 使用 Insert into** 
```text
Insert into Prodcut (prno,prna,qty) Values('001','電腦',100)
Insert into Prodcut (prno,prna,qty) Values('002','鍵盤',100)
Insert into Prodcut (prno,prna,qty) Values('003','滑鼠',100)
```
**各式各樣的 Insert into** 
```text
INSERT INTO Prodcut FROM ARRAY Array1
INSERT INTO Prodcut FROM NAME ObjectName
INSERT INTO Prodcut FROM MEMVAR  
Insert into Prodcut (prno,prna,qty) Selse prno,prna,qty from OtherProduct
```

**紀錄位置的移動**
```text
Select 1
Use Product

Go Top
Go Bottom

Go Top
Skip 2
Skip -1

Go 1
Go 2
Go 3
```
**紀錄滾動 使用 Do While**
```text
Select 1
Use Product
go top 
do whiel !eof()
    =Messagebox(Product.Prno)
    skip 
enddo 
Use 
```
**紀錄滾動 使用 Scan for**
```text
select 1
use Product
Scan 
  =Messagebox(Product.Prno)
endscan 
Use 
```
**資料的搜尋 Locate**
```text
select 1
use Product
Locate for Prno="001"
if !Eof()  
    Messagebox(Product.Prno)
Else 
    Messagebox("找不到")    
endif 
Use 
```
**資料的刪除 Delete**
```text
Set Delete ON 
select 1
use Product
Locate for Prno="001"
if !Eof()  
    Delete 
endif 
Use 
```
**資料的刪除 SQL Delete**
```text
select 1
use Product
Delete From Product Where Prno="001"
Use 
```
**看見被刪除的資料 Set Delete OFF**
```text
Set Delete OFF 
select 1
use Product
Locate for Prno="001"
if !Eof()  
    Delete 
endif 
Use 
```
**救回被刪的資料 Recall**
```text
Set Delete OFF 
select 1
use Product
Locate for Prno="001"
if !Eof()  
    Delete 
endif 

Recall ALL && <<===

Use 
```
**刪除後的資料壓縮整理 Pack**
```text
select 1
use Product Exclusive
Locate for Prno="001"
if !Eof()  
    Delete 
endif 

Pack  && <<===

Set Delete OFF 
Browse 
Set Delete ON 
Browse 

Use 
```
**資料的清空**
```text
select 1
use Product Exclusive
Zap && << 危險 清光光
use 
```
**資料的修改使用 Replace**
```text
select 1
use Product
Locate for Prno="001"
if !Eof()  
    Replace Prna with "桌上電腦"
endif 
```
**資料的修改使用 Scatter Gather** 
```text
Local m.oRecord

select 1
use Product
Locate for Prno="001"
if !Eof()  
    scatter name m.oRecord
endif 

m.oRecord.Prna = "桌上電腦"

Locate for Prno="001"
if !Eof()  
    Gather name m.oRecord
endif 

Browse

use 
```
**資料的修改使用 Update SQL**
```text
select 1
use Product
Browse 

Update Product Set Prna="桌上電腦" where Prno="001"

Browse
use 
```

**索引檔的種類**
```text
1.同名 CDX  && 最常用
2.異名 CDX
3.單索引 IDX
```


**創建索引 CDX**
```text
select 1
use Product Exclusive
index on prno Tag Prno
Index on prna Tag Prna
use 
```
**使用索引 Set Order To 號碼**
```text
select 1
use Product Exclusive
Browse
set order To 1
Browse
set order To 2
Browse
use 
```
**使用索引 Set Order To Tag 名稱**
```text
select 1
use Product Exclusive
Browse
set order To Prno
Browse
set order To Prna
Browse
use 
```
**使用索引搜尋 Seek**
```text
select 1
use Product Exclusive
index on prno Tag Prno
Index on prna Tag Prna

set order to 1 && 指定索引
seek "001"     && 搜尋
if !eof()
  =Messagebox(Prodeuct.Prno)
Else
  =Messagebox("找不到")
endif 
use 
```
**使用索引搜尋 Seek()**
```text
select 1
use Product Exclusive
index on prno Tag Prno
Index on prna Tag Prna

set order to 1 && 指定索引
if seek("001")     && 搜尋 *seek("001","Product") *seek("001","Product","Prno")
  =Messagebox(Prodeuct.Prno)
Else
  =Messagebox("找不到")
endif 
use 
```

**使用索引搜尋 IndexSeek()**
```text
select 1
use Product Exclusive
index on prno Tag Prno
Index on prna Tag Prna

set order to 1 && 指定索引
if IndexSeek("001",.F.,"Product","Prno") && .F. 紀錄指標不要移動 
  ==Messagebox("有找到")
Else
  =Messagebox("找不到")
endif 

Browse

use 
```
**索引重整 Reindex**
```text
select 1
use Product Exclusive
Reindex 
use 
```
**跨越工作區**
```text
* 不同工作區開檔 一
select 1
use Product
Select 2
Use Customer
```
```text
* 不同工作區開檔　二
select 0
use Product
Select 0
Use Customer
```
```text
* 不同工作區開檔　三
use Product in 0
Use Customer in 0
```

**不同工作區新增 Insert Into**
```text
* SQL語法
Insert into Product (Prno,Prna,Qty) Values("004","耳機",400)
Insert into Customer (CuNo,CuNa) Values("A004","聯強國際")

* 一般語法
append blank in Product  
replace prno with "004",Prna with "耳機",Qty with 400   in Product
append blank in Customer
replace CuNo with "A004"CuNao,CuNa with "聯強國際"   in Customer
```
 **不同工作區修改 Update**
 ```text
* SQL語法
update product set Qty=401 where Prno="004"
update Customer set Cuna="聯強國際集團" where CuNo="A004"
* 一般語法
replace Qty  with 401           for Prno="001"  in Product
replace CuNa with "聯強國際集團" for Cuno="A004" in Customer
```
**不同工作區刪除 Delete From** 
```text
* SQL語法
Delete From Product where Prno="004"
Delete From Customer where CuNo="A004"
* 一般語法
Delete for Prno="004" in Product
Delete for Cuno="A004" in Customer
```
**Table之間的關係一**
```text
Create Cursor Teacher(ID C(3),Name C(10))
Insert Into Teacher Values("T01","王老師","T01")
Insert Into Teacher Values("T02","李老師","T02")
Index on Id Tag ID
Browse

Create Cursor Student(ID C(3),Name C(10),TecherID c(3))
Insert Into Student Values("101","張一","T01")
Insert Into Student Values("102","張二","T02")
Insert Into Student Values("103","張三","T02")
Index on Id Tag ID
Browse

Insert Into Student Values("201","李一","T01")
Insert Into Student Values("202","李二","T02")
Insert Into Student Values("203","李三","T02")
Insert Into Student Values("204","李四","T02")

Browse
```
```text
select Student
Scan
  select Teacher
  Seek Student.TecherID
  if eof()
    =Messagebox("找不到")
  Else  
    =Messagebox("學生"+Student.Name +"的老師是"+Teacher.Name)
  endif  
endscan 
close all 
```
```text
*Table之間的關係二
Select Student.ID,Student.Name,Student.Id,Teacher.Name from Student,Teacher where Student.TecherID=Teacher.ID Into cursor Temp0
Select Temp0
Browse 
```
```text
*Table之間的關係三
select Student 
set Relastion to ID Into Teacher additive 
```
**Table 的獨佔與共用**
```text
Select 1
Use Product Exclusive && SHARED
```
**Table 的鎖定**
```text
SET REPROCESS TO 3 SECONDS

Select 1
Use Product 
* 鎖定一筆資料
locate for Prno="001"
if RLock()
  Replace Qty with Qty+1  
Else
  =Messagebox("無法鎖定有人使用中")
endif 
UnLock
* 鎖定檔案
if FLock()
  Update Prodcut set Qty = Qty+1 where Prno="001"
Else
  =Messagebox("無法鎖定有人使用中")
endif 
UnLock
```

**練習**

\*\*\*\*

