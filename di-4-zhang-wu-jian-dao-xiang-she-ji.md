# 第4章 物件導向設計

**件介紹** 

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
* 物件的三個功能\(封裝、繼承、多型\)
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

```bash
*新增資料夾
MD C:\vfp\Lesson4\

* 轉換當前資料夾
Set Default to C:\vfp\Lesson4\

*創建新專案
Create Project proj4 && 記得按存檔

```

**專案新增 Form**

```bash
*使用指令新增Form

* 轉換當前資料夾
Set Default to C:\vfp\Lesson4\

*修改當前專案
Modify Project proj4 

* 加入新的Form
Create Form Form0 && 記得存檔並且加入proj4 -> Documents -> Form 
Create Form Form1 && 記得存檔並且加入proj4 -> Documents -> Form 
Create Form Form2 && 記得存檔並且加入proj4 -> Documents -> Form 

Do Form Form0
Do Form Form1
Do Form Form2
```

**簡單的物件**

```bash
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

```bash
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

```bash
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

```bash
* 簡單的自訂物件1

LOCAL oSimpleObject

oSimpleObject = CREATEOBJECT("SimpleObject")

oSimpleObject.cYourName = "John"

oSimpleObject.SayHello()

RELEASE oSimpleObject && 釋放

CLEAR ALL && 釋放所有


*繼承自Custom客製化自訂義
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

```bash
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

**物件的封裝**

```bash

* 物件的封裝

LOCAL oStringToos

oStringToos=CREATEOBJECT("StringToos")
oStringToos.Append("你好 ! ")
oStringToos.Append("我喜歡使用物件，")
oStringToos.Append("希望你也喜歡。")
oStringToos.SayString()                  && 你好 !...
=MESSAGEBOX( oStringToos.GetString()  )  && 你好 !... 

oStringToos.Clear()
oStringToos.Append("再見啦。")
oStringToos.SayString()                  && 再見啦。
=MESSAGEBOX( oStringToos.GetString()  )  && 再見啦。 

LOCAL oSquare

oSquare=CREATEOBJECT("Square")
oSquare.x1 = 0
oSquare.y1 = 0
oSquare.x2 = 10
oSquare.y2 = 10
=MESSAGEBOX("面積等於： "+ TRANSFORM( oSquare.Area() )  )       && 面積等於：100
=MESSAGEBOX("週長等於： "+ TRANSFORM( oSquare.Perimeter() )  )  && 週長等於：40


* 字串工具類
DEFINE CLASS StringTools as Custom 
	cStr = ""	&& 屬性

	FUNCTION Append(A)        && 串接字串
		This.cStr= This.cStr + A		
	ENDFUNC 
	
	FUNCTION Clear()       &&  清除字串
		This.cStr= ""
	ENDFUNC 
	
	FUNCTION SayString()    && 顯示字串
		=MESSAGEBOX( This.cStr )
	ENDFUNC 
	
	FUNCTION GetString()     && 回傳字串
		RETURN  This.cStr 
	ENDFUNC 
		
ENDDEFINE 

* 矩形類
DEFINE CLASS Square  as Custom
	X1 = 0
	Y1 = 0
	
	x2 = 0
	Y2 = 0	

	FUNCTION Area() && 面積
		
		RETURN ABS( (this.x1-This.x2) * (This.y1-This.y2) )
	ENDFUNC 
	
	FUNCTION Perimeter()  && 週長
		RETURN ABS( (this.x1-this.x2)*2) + ABS((this.y1-this.y2)*2 )
	ENDFUNC 
	
ENDDEFINE 

CLEAR ALL 

```

**物件的繼承**

```bash
* 物件的繼承

LOCAL oAnimal,oMonkey,oMenkind

* 動物有一種本事
oAnimal=CREATEOBJECT("Animal") 
oAnimal.Run()

* 猴子有兩種本事
oMonkey=CREATEOBJECT("Monkey")
oMonkey.Run()
oMonkey.UseTools()

* 人類有三種本事
oMenkind=CREATEOBJECT("oMenkind")
oMenkind.Run()
oMenkind.UseTools()
oMenkind.GoToMoon()

* 動物有一種本事 到處亂跑
DEFINE CLASS Animal as Custom 
		
	FUNCTION Run()
		=MESSAGEBOX("I Can Run")
	ENDFUNC 
	
ENDDEFINE 

* 猴子繼承動物但又多了一項本事 使用工具
DEFINE CLASS Monkey as Animal 
		
	FUNCTION UseTools()
		=MESSAGEBOX('I Can Use Tools')
	ENDFUNC 
	
ENDDEFINE 

* 人類繼承猴子但又多了一項本事 登陸月球
DEFINE CLASS oMenkind as Monkey
		
	FUNCTION GoToMoon()
		=MESSAGEBOX('I Can GoToMoon')
	ENDFUNC
		
ENDDEFINE 

CLEAR ALL 

```

**物件的多型**

```bash
* 物件的多型

LOCAL oPerson,oCat,oDog


oCat=CREATEOBJECT("Cat")

oDog=CREATEOBJECT("Dog")


oPerson=CREATEOBJECT("Person")
oPerson.MyPet = oDog
oPerson.PetSayHello() &&

oPerson.MyPet = oCat
oPerson.PetSayHello() && 


DEFINE CLASS person as Custom
	MyPet = Null
		
	FUNCTION PetSayHello()
		this.MyPet.SayHello()	
	ENDFUNC 
	
ENDDEFINE 


DEFINE CLASS Dog as Custom
		
	FUNCTION SayHello()
		=MESSAGEBOX('汪')
	ENDFUNC 
	
ENDDEFINE 


DEFINE CLASS Cat as Custom
		
	FUNCTION SayHello()
		=MESSAGEBOX('喵')
	ENDFUNC
		
ENDDEFINE 

CLEAR ALL 


```

**物件的生成與消滅 Event**

```bash
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

```bash
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

```bash
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

```bash
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

```bash
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

```bash
* 在記憶體中產生 Table 

CREATE CURSOR Product (Prno C(10), Prna C(20) , Qty N(18,0))
APPEND BLANK
REPLACE Prno WITH "001",Prna WITH "電腦",Qty WITH 1
APPEND BLANK
REPLACE Prno WITH "002",Prna WITH "鍵盤",Qty WITH 2

BROWSE

* 把兩筆資料轉成兩個 空白物件 
LOCATE FOR Prno = "001" && 定位
SCATTER NAME oEmpty1 && 產生物件 複製一筆資料<======

LOCATE FOR Prno = "002" && 定位
SCATTER NAME oEmpty2 && 產生物件 複製一筆資料 <======

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
GATHER NAME oEmpty1 && 寫回第一筆 <======

LOCATE FOR Prno = "002" && 定位
GATHER NAME oEmpty2 && 寫回第二筆 <======


BROWSE && 瀏覽


RELEASE m.oEmpty1,m.oEmpty2 && 釋放 

CLEAR ALL  && 全部釋放 

Close Tables ALL && 關閉所有 Table

RETURN 



```

**容器物件**

```bash
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

```bash
* 容器物件 配合 空白物件 配合 Table  範例
*僅供參考

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

```text
* 使用 Do Form

專案上存三個 Form 分別為 Form0 Form1 Form2

主程式 Do Form0

Form0 上加上兩個按鈕分別以do form 呼叫另外兩個Form 

```

**練習二**

```text
* 使用物件庫

專案上建立物件庫檔名為ClassLib1
物件庫中加入三個繼承Form類別分為Form0 Form1 Form2 
主程式設定使用ClassLib1
並使用Createobject 以FORM0為類生成Local oForm 並顯示
修改FORM0類加上兩顆按鈕分別以Createobject 生成與顯示另外兩個Form類

```

**練習三**

```text
* 物驗的封裝

學習封裝。模仿串字串工具類與矩形類，白手起家。

```

練習四

```text
* 物件的繼承


學習繼承，使用define Class 定義四代計算機
每一代都是繼承上一代
每一代都多了一種計算方式

第一代有兩數相加
第二代有兩數相減
第三代有兩數相乘
第四代有兩數相除

到了第四代就能真正有加減乘除四種方法

程式只要生成第四代物件並測試是否具有四種計算能力

```

**練習五**

```text
* 物件的多型

學習物件的多型，模仿帶寵物，寵物叫
```

**練習六**

```text
* 空白物件與容器物件

製作三顆Empyt 物件代表三個學生

每顆其屬性類都有：學號、名稱、年紀

生成一個容器並投入三顆Empty物件以學號為Key

測試時以第二個學生的Key由容器找回第二顆Empty物件

讀取該物件屬性看是否真為第二個學生的資料

```

**練習七**

```bash
* 複數的範例一

Local oAA,oBB,oCC  

oAA=CREATEOBJECT("RI",1,3) && １＋３ｉ

oBB=CREATEOBJECT("RI",2,4) && ２＋４ｉ

oCC=Add(oAA,oBB) && 兩複數物件相加
oCC.SayHello() &&３＋７ｉ

CLEAR ALL && 釋放所有變數

*******************
FUNCTION Add(oA,oB)&&這個ADD沒有被封裝???
*******************
	LOCAL nR,nI,oC
	nR=oA.RR+oB.RR
    nI=oA.II+oB.II
    oC = CREATEOBJECT("RI",nR,nI)
	RETURN oC
ENDFUNC


* 複數物件 1+2i
DEFINE CLASS RI as Custom
	RR =0 && 實部
	II =0 && 虛部
	
	FUNCTION init(nRR,nII) && 生成給值
		this.RR=nRR
		this.II=nII
	ENDFUNC 
	
	
	FUNCTION SayHello()	
		=MESSAGEBOX( TRANSFORM(This.RR)+ "+" +TRANSFORM(This.II)+"i")
	ENDFUNC 
		
ENDDEFINE 

```

```text
* 複數的範例 二

Local oAA,oBB,oCC  

oAA=CREATEOBJECT("RI",1,3) && １＋３ｉ

oBB=CREATEOBJECT("RI",2,4) && ２＋４ｉ

oCC=CREATEOBJECT("RI",0,0) && ０＋０ｉ
oCC.Clear()    && 清除內容
oCC.Add(oAA)   && 累加
oCC.Add(oBB)   && 累加
oCC.SayHello() && ３＋７ｉ

oCC.Add2(oAA,oBB) && 兩複數物件相加封裝了範例一的ADD
oCC.SayHello() && ３＋７ｉ

CLEAR ALL


*複數物件類 1+2i
DEFINE CLASS RI as Custom
	RR =0 && 實部
	II =0 && 虛部
	
	FUNCTION init(nRR,nII) && 生成給值
		this.RR=nRR
		this.II=nII
	ENDFUNC 
	
	FUNCTION Clear() && 清除內容
		this.RR = 0
		this.II = 0
	ENDFUNC 
	
	FUNCTION Add(oA) && 累加複數物件
		This.RR = This.RR + oA.RR
		this.II = This.II + oA.II
	ENDFUNC
				
	FUNCTION Sub(oA) && 累減複數物件
		This.RR = This.RR - oA.RR
		this.II = This.II - oA.II
	ENDFUNC 
	
	FUNCTION SayHello() && 看內容
		=MESSAGEBOX(TRANSFORM(this.RR)+ "+" +TRANSFORM(This.II)+"i")
	ENDFUNC 
	
	FUNCTION Add2(oA,oB) && 兩複數物件相加
		This.RR = oA.RR + oB.RR
		this.II = oA.II + oB.II
	ENDFUNC
	
ENDDEFINE 

```

```text
*件導向是一種解決問題的思考方式
*解決問題的方法不只一種，你能想出還有什麼問題可以用物件導向方式解決?
```

