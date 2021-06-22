# 第4章 物件導向設計

**表單元件** 

* 屬性
* 方法 
* 事件

**元件庫**

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

**物件庫 Classlib**

```text
* 使用物件庫新增 Form

CREATE CLASSLIB myclslib1     && 新增物件庫 .VCX 

CREATE CLASS TForm1 OF myclslib1 AS "Form"  && 新增類 "TForm1" 祖先類是 "Form"
CREATE CLASS TForm2 OF myclslib1 AS "Form"  && 新增類 "TForm2" 祖先類是 "Form"

SET CLASSLIB TO myclslib1 ADDITIVE     && 引用物件庫
Local oForm1,oForm2
oForm1=CreateObject("TForm1")
oForm1.Caption="我的畫面1"
oForm1.Show(1)

oForm2=CreateObject("TForm2")
oForm1.Caption="我的畫面2"
oForm2.Show(1)

```

**類別與物件**

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
		=Messagebox("我叫 :" + this.cName + " 我是一隻：" + this.cType )
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

**練習**



