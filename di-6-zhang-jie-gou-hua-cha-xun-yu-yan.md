# 第6章 結構化查詢語言

**SQL概述**

* SQL的特點
* SQL的差異

**資料查詢**

* SELECT命令
* 簡單查詢
* 特殊運算符
* 統計查詢
* 分組查詢
* 排序查詢
* Jonnig 查詢
* Inner Outer Jonnig 查詢
* 嵌套查詢
* Exists Any All Some
* 集合運算
* 輸出查詢結果

**操作資料**

* Insert 新增
* Update 修改
* Delete 刪除

**資料定義**

* 建立資料表
* 修改資料表
* 刪除資料表

**SQL 語句**

```text
* Create Table ...
* Alter Table ...
* Drop Table ...

* Insert ... 
* Update ... 
* Delete ...

* Select ...

```

**測試資料**

```text

SET DATE TO YMD
SET CENTURY ON 

* 客戶檔(客戶代號 ,客戶名稱)
Create Cursor Cust(CustNo c(3),CustNa c(10)) && 客戶廠商檔
Insert into Cust Values( "C01" ,"台積電" )
Insert into Cust Values( "C02" ,"鴻海"   )
Insert into Cust Values( "C03" ,"聯發科" )
Browse

* 銷貨單表頭檔(單號 ,客戶代號 ,銷售日期)
Create Cursor Billm(No c(5),CustNo c(3),BDate D) 
Insert into Billm Values( "KB001" ,"C01" ,Date(2021,1,1) )
Insert into Billm Values( "KB002" ,"C02" ,Date(2021,1,2) )
Insert into Billm Values( "KB003" ,"C03" ,Date(2021,1,2) )
Insert into Billm Values( "KB004" ,"C01" ,Date(2021,1,3) )
Insert into Billm Values( "KB005" ,"C02" ,Date(2021,1,3) )
Browse

* 銷貨單表身檔(單號 ,項次 ,品號 ,品名 ,數量 ,單價 ,金額) 
Create Cursor Billd(No c(5),Item N(3),Prno c(3),Prna c(10),Qty n(18),Price n(18),Amt n(18)) 
Insert into Billd Values( "KB001" ,1 ,"P01" ,"電腦" ,1 ,10000 ,10000)
Insert into Billd Values( "KB001" ,2 ,"P02" ,"鍵盤" ,1 ,1000  ,1000)
Insert into Billd Values( "KB001" ,3 ,"P03" ,"滑鼠" ,1 ,100   ,100)
Insert into Billd Values( "KB002" ,1 ,"P01" ,"電腦" ,2 ,10000 ,20000)
Insert into Billd Values( "KB002" ,2 ,"P02" ,"鍵盤" ,2 ,1000  ,2000)
Insert into Billd Values( "KB002" ,3 ,"P03" ,"滑鼠" ,2 ,100   ,200)
Insert into Billd Values( "KB003" ,1 ,"P01" ,"電腦" ,3 ,10000 ,30000)
Insert into Billd Values( "KB003" ,2 ,"P02" ,"鍵盤" ,3 ,1000  ,3000)
Insert into Billd Values( "KB003" ,3 ,"P03" ,"滑鼠" ,3 ,100   ,300)
Insert into Billd Values( "KB004" ,1 ,"P01" ,"電腦" ,4 ,10000 ,40000)
Insert into Billd Values( "KB004" ,2 ,"P02" ,"鍵盤" ,4 ,1000  ,4000)
Insert into Billd Values( "KB004" ,3 ,"P03" ,"滑鼠" ,4 ,100   ,400)
Insert into Billd Values( "KB005" ,1 ,"P01" ,"電腦" ,5 ,10000 ,50000)
Insert into Billd Values( "KB005" ,2 ,"P02" ,"鍵盤" ,5 ,1000  ,5000)
Insert into Billd Values( "KB005" ,3 ,"P03" ,"滑鼠" ,5 ,100   ,500)
Browse


```

**建立資料表**

```text
Create Table|DBF Table1 [Name <長名稱>] [Free] ;  && 如果不是自由表 必須先開啟資料庫
( ;
  Prno C(3),   ;
  Prna C(10),  ;
  Qty  N(18)   ;
)
```

**修改資料表**

```text
Alter Table <表名> Add Column <新欄名> C(10)
Alter Table <表名> Alter Column <舊欄名> C(10)
Alter Table <表名> ReName Column <舊欄名> to <新欄名>
Alter Table <表名> Drop Column <舊欄位>
```

**刪除資料表**

```text
Drop Table <表名> && 必須先開啟資料庫
```

**輸出查詢結果**

```text
Select ... Into Cursor <游標名> ReadWrite
Select ... Into Array  <陣列名> 
Select ... Into Table c:\vfp\...\abc.dbf
Select ... To File c:\vfp\...\abc.txt
Select ... To Printer
```

**SQL新增**

```text
Insert into <表名> (<Field1>,<Field2>) Values (<Value1>,<Value2>)
Insert into <表名> Values (<Value1>,<Value2>)
Insert into <表名> From Array <陣列名>
Insert into <表名> From MemVar 
Insert into <表名> From Name <物件名>
```

**SQL修改**

```text
Update <表名> Set <欄位1> = <值1>  ,<欄位2> = <值2> Where <邏輯運算式>
```

**SQL刪除**

```text
Delete From <表名> Where <邏輯運算式>
```

**簡單查詢**

```text
Select * From Product Where Prno>='002'
Select Prno,Prna,Qty From Product Where Prno>='002'
```

**不重複 Distinct**

```text
Select Distinct Prno From Product Where Prno>='002'
```

**欄位別名**

```text
Select prno as aa,Prna as bb Form Product Where Prno>='002'
```

**資料表別名**

```text
Select Product.Prno ,Product.Prna Form Product Where Prno>='002'
Select PP.Prno ,PP.Prna Form Product PP Where Prno>='002'
```

**包含 IN**

```text
Select * From Product Where Qty in (100,101,102)
Select * From Product Where Prna in ('電腦','鍵盤','滑鼠')
```

**介於**

```text
Select * From Product Where Qty Between 100 And 200 && 類似 Qty>=100 And Qty<=200  
Select * From Product Where Not Qty Between 100 And 200 && 可以相反
```

**類似於**

```text
Select * From Product Where Prna Like "電%" && 開頭是"電"
Select * From Product Where Left(Prna,2)="電"
```

**排序查詢**

```text
Select * From Product Order By Prno
Select * From Product Order By Qty Asc
Select * From Product Order By Qty Desc
```

**統計查詢**

```text
Select Count(*) from Product Where Prno>='002'
Select Sum(Qty) from Product Where Prno>='002'
Select Max(Qty) from Product Where Prno>='002'
Select Min(Qty) from Product Where Prno>='002'
Select AVG(Qty) from Product Where Prno>='002'
```

**分組查詢**

```text
Select CustomerID,Count(*) as 筆數,Max(Qty) as 最大額 From SalesList Group By CustomerID
Select CustomerID,Count(*) as 筆數,Max(Qty) as 最大額 From SalesList Group By CustomerID Having CustomerID > "000"
```

**嵌套查詢**

```text
Select Accno From Table1 Where Accno in (Select Accno from Table2 )
```

**多表聯合 Union**

```text
Select Prno,Prna Form Table1 ;
Union ;
Select Prno,Prna Form Table2
```

**交互連結 Jonnig**

```text
Select Bill,Date,Item,Prno,Prna,Qyt,Price,Amt from Billd,Billm where Billd.no=Billm.no
```

**超級交互連結** Inner \| Left \| Right \| FULL\(Outer\)

```text
Select ... From Table0 ;
  Left Join Table1 On Table0.no1 = Table1.no1 ;
  Left Join Table2 On Table0.no2 = Table2.no2 ;
```

**存在\|有任何\|全部** Exists\|Any\(Some\)\|All

```text
Select ... From Table1 Where Exists (Select ... From Table2 Table1.no=Table2.no)
Select ... From Table1 Where Not Exists (Select ... From Table2 Table1.no=Table2.no)
Select ... From Table1 Where Any (Select ... From Table2 Table1.no=Table2.no)
Select ... From Table1 Where All (Select ... From Table2 Table1.no=Table2.no)
```

**練習**

```text
...
```

