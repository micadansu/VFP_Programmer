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

**專案新增物件庫 Classlib並於物件庫中新增Form物件**

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

**練習**



