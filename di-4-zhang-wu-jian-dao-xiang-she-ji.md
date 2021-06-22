# 第4章 物件導向設計

**表單元件介紹** 

* 屬性
* 方法 
* 事件

**元件庫介紹**

* 基礎物件
* 新增物件
* 元件庫的引用
* 使用元件

**自定義物件** 

* 簡單的物件
* 物件的初始 
* 物件的結束 
* 屬性
* 方法

**空白物件** 

* 由程式產生
* 由資料表產生

**容器物件**

* 加入項目
* 引用項目
* 移除項目
* 項目也可以是物件
* 複製表整個資料表

**新增專案**

```text
MD C:\vfp\Lesson4\

Set Default to C:\vfp\Lesson4\

Create Project proj4 && 記得按存檔

```

**專案新增 Form**

```text
*使用指令新增Form


Modify Project proj4 

Create Form Form1 && 記得存檔並且加入proj4 -> Documents -> Form 
Create Form Form2 && 記得存檔並且加入proj4 -> Documents -> Form 

Do Form Form1
Do Form Form2
```

**簡單的物件**

```text
* 簡單的物件 1修
* 改屬性的方式

LOCAL oForm as Form

oForm = CREATEOBJECT("Form") && 物件生成
oForm.Caption ="簡單的物件"    && 視窗抬頭
oForm.AutoCenter = .T.       && 自動置中
oForm.Width  = 200           && 寬度
oForm.Height = 200           && 高度
oForm.Show(1)                &&  顯示

RELEASE oForm

RETURN 

```

```text
* 簡單的物件 2 
* 修改屬性使用 with 

LOCAL oForm as Form

oForm = CREATEOBJECT("Form") && 物件生成
WITH oForm 
    .Caption ="簡單的物件" && 視窗抬頭
	.AutoCenter = .T.      && 自動置中
	.Width  = 200          && 寬度
	.Height = 200          && 高度
	
    .AddObject("按鈕1","commandButton")	&& 加入一顆按鈕物件	    
    WITH .按鈕1
		.Width  = 100  && 寬度
		.Height = 50   && 高度
		.Top    = 10   && 上邊界
		.Left   = 10   && 左邊界 
      .Visible = .T. && 看得見                     
	ENDWITH 
			
	.Show(1) && 顯示
ENDWITH 

RELEASE oForm

RETURN 

```

**物件庫 Classlib**

```text
* 使用物件庫新增 Form

CREATE CLASSLIB myclslib1     && 新增物件庫 .VCX 

CREATE CLASS TForm1 OF myclslib1 AS "Form"  && 新增類 "TForm1" 祖先類是 "Form"
CREATE CLASS TForm2 OF myclslib1 AS "Form"  && 新增類 "TForm2" 祖先類是 "Form"

* 物件使用方式

SET CLASSLIB TO myclslib1 ADDITIVE     && 引用物件庫

Local oForm1,oForm2

oForm1=CreateObject("TForm1")
oForm1.Caption="我的畫面1"
oForm1.Show(1)

oForm2=CreateObject("TForm2")
oForm1.Caption="我的畫面2"
oForm2.Show(1)

```

**簡單的自訂物件**

```text
* 簡單的自訂物件1

LOCAL oSimpleObject

oSimpleObject = CREATEOBJECT("SimpleObject")

oSimpleObject.cYourName = "John"

oSimpleObject.SayHello()

RELEASE oSimpleObject && 釋放

CLEAR ALL && 釋放所有


********************************
Define Class SimpleObject As Custom  && 自訂 簡單物件 類
********************************
   * 屬性 Property 就是物件的變數
   cYourName = ""

   * 方法 Method 就是物件的函數
   *******************
   FUNCTION SayHello()
   *******************
       =MESSAGEBOX("Hello " + This.cYourName) && 屬性要加 This.  
   ENDFUNC  
   

ENDDEFINE

```

```text
* 簡單的自訂物件 2

LOCAL oForm
oForm =CREATEOBJECT("MyForm")
oForm.Show(1)

RELEASE oForm

*******************************
DEFINE CLASS MyForm AS form
	
	* 屬性
	Caption = "Form" && 抬頭
	Height =200      && 高
	Width = 100      && 寬
	AutoCenter = .T. && 置中
	
    * 加入按鈕物件
	ADD OBJECT command1 AS commandbutton WITH ;
		Top = 10, ;
		Left = 10, ;
		Height =50, ;
		Width = 100, ;
		Caption = "Command1", ;
		Name = "Command1"

    * 按鈕事件
	PROCEDURE command1.Click
		=MESSAGEBOX("OK")
	ENDPROC

ENDDEFINE
*******************************

```

**物件的生成與消滅 Event**

```text
* 物件的 Event

LOCAL oCaculator

oCaculator=CREATEOBJECT("Caculator")
nAnsAdd = oCaculator.Add(10,20)
nAnsSub = oCaculator.Sub(10,20)

=MESSAGEBOX(nAnsAdd)
=MESSAGEBOX(nAns)


=MESSAGEBOX('準備釋放物件了')

RELEASE oCaculator && 釋放物件 也可以這樣會釋放 oCaculator=NULL  

=MESSAGEBOX('物件已經釋放了')

CLEAR ALL && 釋放所有變數


********************************
Define Class Caculator As Custom  && 自訂 計算機 類
********************************

   *******************
   FUNCTION Init()
   *******************
       =MESSAGEBOX("發生事件：物件之生成")   
   ENDFUNC  
   
   *******************
   FUNCTION Destroy()
   *******************
       =MESSAGEBOX("發生事件：物件之消滅")   
   ENDFUNC  


	*******************
	Function Add(xx,yy) 
	******************
		Return xx + yy
	ENDFUNC

	*******************
	Function Sub(xx,yy) 
	******************
		Return xx - yy
	ENDFUNC
	
    ***************
	Function Muti() 
    ***************
	LParameters  xx,yy 
		Return xx * yy
	ENDFUNC
	
    ***************
	Function Div() 
    ***************
	LParameters  xx,yy 
		Return xx / yy
	ENDFUNC

ENDDEFINE

```

**物件生成時於初始事件中傳入參數**

```text
* 物件生成時 可以順便輸入參數 

Local oForm
oForm =Createobject("MyForm","傳票輸入",200,200)
oForm.Show(1)

Release oForm

*******************************
Define Class MyForm As Form

	************************
	Function Init(cText,H,W) && 物件生成時 取得參數
	************************
		This.Caption=cText
		This.Height = H && 高
		This.Width  = W && 寬
	Endfunc

ENDDEFINE
*******************************

```

**物件的方法與屬性** 

```text
* 自定 類別 與 物件

Local oAnimal,oDog,oCat1,oCat2 

oAnimal = Createobject("Animal") && 使用動物類 產生物件
oAnimal.SayName()
oAnimal.SayHello()
oAnimal.SayGoodBye()

oDog = Createobject("Dog") && 使用狗類 生成物件
oDog.cName = "來福"
oDog.nAge = 2
oDog.cType= "金毛犬" 

oDog.SayName()
oDog.SayHello()
oDog.SayGoodBye()

oCat1 = Createobject("Cat") && 使用貓類 產生貓1
WITH oCat1
	.cName = "毛球"
	.nAge = 1
	.cType= "波絲貓" 
	
	.SayName()	
	.SayHello()
	.SayGoodBye()
ENDWITH 

oCat2 = Createobject("Cat") && 使用貓類 產生貓2
WITH oCat2 
	.ChangeData("小藍",1,"俄國藍貓" ) && 改屬性	
	
	.SayHello()
	.SayName()
	.SayGoodBye()
ENDWITH 

cMyPet = oDog.cName + " " +oCat1.cName + " " + oCat2.cName 
=MESSAGEBOX("我的寵物：" +  cMyPet)

CLEAR ALL && 清除記憶體



*****************************
Define Class Animal As Custom && 動物類 繼承 Custom

    * 屬性 Property
    
	cName = "???" && 動物名子
	nAge  = 0     && 動物年齡
	cType = "???" && 動物品種
	
	* 方法 Method
	
	Function SayHello()
		=Messagebox("你好")
	ENDFUNC
		
	Function SayGoodbye()
		=Messagebox("再見")
	ENDFUNC
		
	FUNCTION ChangeData(N,A,T) && 改屬性
		This.cName = N
		This.nAge  = A
		This.cType = T	  	
	ENDFUNC 
	
	Function SayName()
	  =Messagebox("我叫 :" + this.cName + " 我是一隻：" + this.cType)
	ENDFUNC
				
ENDDEFINE 



*****************************
Define Class Dog As Animal && 狗類 繼承 動動物類

	Function SayHello()
		=Messagebox("汪汪汪(你好)")
	Endfunc

	Function SayGoodbye()
	  	=Messagebox("汪汪汪(再見)")
	ENDFUNC
		
ENDDEFINE 



*****************************
Define Class Cat As Animal && 貓類 繼承動物類

	Function SayHello()	
		=Messagebox("喵喵喵(你好)")
	Endfunc

	Function SayGoodbye()
	  	=Messagebox("喵喵喵(再見)")
	ENDFUNC
	
	
ENDDEFINE 
*****************************

```

**由程式產生 空白物件 Empty**

```text
* 空白物件

LOCAL m.oEmpty1,m.oEmpty2


m.oEmpty1= CREATEOBJECT( "Empty" )

ADDPROPERTY( oEmpty1 , "Prno" , "001"  )
ADDPROPERTY( oEmpty1 , "Prna" , "電腦" )
ADDPROPERTY( oEmpty1 , "Qty"  , 100    )



m.oEmpty2= CREATEOBJECT( "Empty" )

ADDPROPERTY( oEmpty2 , "Prno" , "002"  )
ADDPROPERTY( oEmpty2 , "Prna" , "鍵盤" )
ADDPROPERTY( oEmpty2 , "Qty"  , 100    )


=MESSAGEBOX( m.oEmpty1.Prno +" "+ m.oEmpty1.Prna) && 01 電腦

=MESSAGEBOX( m.oEmpty2.Prno +" "+ m.oEmpty2.Prna) && 02 鍵盤


WITH m.oEmpty1
	=MESSAGEBOX( .Prno +" "+ .Prna) && 01 電腦
ENDWITH 

WITH m.oEmpty2
	=MESSAGEBOX( .Prno +" "+ .Prna) && 02 鍵盤
ENDWITH 

RELEASE m.oEmpty1,m.oEmpty2 

CLEAR ALL

```

**由資料表產生 空白物件**

```text
* 在記憶體中產生 Table 

CREATE CURSOR Product (Prno C(10), Prna C(20) , Qty N(18,0))
APPEND BLANK
REPLACE Prno WITH "001",Prna WITH "電腦",Qty WITH 100
APPEND BLANK
REPLACE Prno WITH "002",Prna WITH "鍵盤",Qty WITH 200

BROWSE

* 把兩筆資料轉成兩個 空白物件 
LOCATE FOR Prno = "001" && 定位
=MESSAGEBOX( Prno +" "+ Prna) && 01 電腦
SCATTER NAME oEmpty1 && 產生物件複製一筆資料

LOCATE FOR Prno = "002" && 定位
=MESSAGEBOX( Prno +" "+ Prna) && 02 鍵盤
SCATTER NAME oEmpty2 && 產生物件複製一筆資料


* 利用物件 顯示內容 很像是影印了兩筆資料
WITH m.oEmpty1
	=MESSAGEBOX( .Prno +" "+ .Prna) && 01 電腦
ENDWITH 

WITH m.oEmpty2
	=MESSAGEBOX( .Prno +" "+ .Prna) && 02 鍵盤
ENDWITH 


RELEASE m.oEmpty1,m.oEmpty2 

CLEAR ALL

Close Tables ALL && 關閉所有 Table

RETURN 

```

**修改空白物件寫回 Table**

```text
* 在記憶體中產生 Table 

CREATE CURSOR Product (Prno C(10), Prna C(20) , Qty N(18,0))
APPEND BLANK
REPLACE Prno WITH "001",Prna WITH "電腦",Qty WITH 1
APPEND BLANK
REPLACE Prno WITH "002",Prna WITH "鍵盤",Qty WITH 2

BROWSE

* 把兩筆資料轉成兩個 空白物件 
LOCATE FOR Prno = "001" && 定位
SCATTER NAME oEmpty1 && 產生物件 複製一筆資料

LOCATE FOR Prno = "002" && 定位
SCATTER NAME oEmpty2 && 產生物件 複製一筆資料

* 修改物件上的 Qty
oEmpty1.Qty = 101
oEmpty2.Qty = 201

* 使用物件 顯示修改 
WITH m.oEmpty1
	=MESSAGEBOX( .Prno +" "+ .Prna +" " + TRANSFORM(.Qty) ) && 01 電腦
ENDWITH 

WITH m.oEmpty2
	=MESSAGEBOX( .Prno +" "+ .Prna+ " " + TRANSFORM(.Qty)) && 02 鍵盤
ENDWITH 


* 物件寫回 Table
LOCATE FOR Prno = "001" && 定位
GATHER NAME oEmpty1 && 寫回第一筆 

LOCATE FOR Prno = "002" && 定位
GATHER NAME oEmpty2 && 寫回第二筆 


BROWSE && 瀏覽


RELEASE m.oEmpty1,m.oEmpty2 && 釋放 

CLEAR ALL  && 全部釋放 

Close Tables ALL && 關閉所有 Table

RETURN 



```

容器物件

```text
LOCAL oCollection as Collection && as Collection 可省略

oCollection = CREATEOBJECT("Collection") && 容器物件

* 加入兩個 Value
oCollection.Add("小明","001") && (內容,健值) (Value,Key)
oCollection.Add("小智","002") && (內容,健值) (Value,Key)

* 使用 Key 取出 Value
LOCAL cName1,cName2
cName1=oCollection.Item("001")
cName2=oCollection.Item("002")
=MESSAGEBOX( cName1 ) && "小明"
=MESSAGEBOX( cName2 ) && "小智"

* 使用 Key 查位置(第幾個)
LOCAL nIndex
nIndex  = oCollection.GetKey("001")	
=MESSAGEBOX( nIndex ) && 1
nIndex  = oCollection.GetKey("002")	
=MESSAGEBOX( nIndex ) && 2


* 使用迴圈 遍訪 Value
nCount = oCollection.Count && 容器內總共有有幾個 2

LOCAL cName
FOR i = 1 TO nCount
	cName = oCollection.Item( i ) && 位置找 Value
	=MESSAGEBOX( cName ) && "小明"、"小智"...
ENDFOR 

* 使用迴圈 遍訪Key值
LOCAL cKey
FOR i = 1 TO nCount
	cKey = oCollection.GetKey( i ) && 位置找 Key	
	=MESSAGEBOX( cKey ) && "001"、"002" ...
ENDFOR 

RELEASE oCollection

```

**容器物件複雜的範例**

```text
* 容器物件 配合 空白物件 配合 Table  範例

LOCAL oCollection as Collection && as Collection 可省略
LOCAL oRec,cKey && Empty
LOCAL oEmpty1

oCollection = CREATEOBJECT("Collection") && 容器物件

* 在記憶體中產生 Table 
CREATE CURSOR Product (Prno C(10), Prna C(20) , Qty N(18,0))
APPEND BLANK
REPLACE Prno WITH "001",Prna WITH "電腦",Qty WITH 1
APPEND BLANK
REPLACE Prno WITH "002",Prna WITH "鍵盤",Qty WITH 2
BROWSE

* 滾動 Table 將資料 一筆一筆加入容器

SELECT Product  && 指定 Table
GO TOP          && 跳到開頭第一筆
DO WHILE !EOF() && 直到檔案結束 End of File
   SCATTER NAME oRec   && 產生 Empty 物件，含有所有欄位屬性  
   
   && 品號作為 Key 注意長度有10碼 "001       "   
   cKey = Product.Prno 
      
   oCollection.Add(oRec , cKey) && 投入容器 ( Value , Key )   
   SKIP && 跳下一筆   
ENDDO 

* 使用 Key 取出第一個物件
cKey=PADR("001",10) && 補足10 碼，必須配合當初投入容器做 Key 的長度
LOCAL m.oEmpty
oEmpty = oCollection.Item( cKey ) && 取出內容 當初投入是 物件
WITH m.oEmpty
	=MESSAGEBOX(  .Prno+" "+.Prna+" "+ TRANSFORM(.Qty)) && 測試物件屬性
ENDWITH 

* 使用 Key 取出第二個物件
cKey=PADR("002",10) && 補足10 碼，必須配合當初投入容器做 Key 的長度
LOCAL m.oEmpty
oEmpty = oCollection.Item( cKey ) && 取出內容 當初投入是 物件
WITH m.oEmpty
	=MESSAGEBOX(  .Prno+" "+.Prna+" "+ TRANSFORM(.Qty)) && 測試物件屬性
ENDWITH 



RELEASE oCollection


RETURN 


```

**練習一**

**練習二**

**練習三**

