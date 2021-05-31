# 第2章 資料型態

**常數與變數**

* 註解
* 變數
* 常數
* 陣列

**註解**

```text
* 這是註解

&&這個是註解
```

**變數**

```bash
* 變數 經常會使用 習慣大小寫混用 

VIP_No = "A01"
VIP_Na = "張忠某"

=MESSAGEBox("卡號 : " + VIP_No ) 

=MESSAGEBox("姓名 : " + VIP_Na ) 


* 變數 可改變其內容

VIP_No = "A02"
VIP_Na = "郭台明"

=MESSAGEBox("卡號 : " + VIP_No ) 

=MESSAGEBox("姓名 : " + VIP_Na ) 


```

**常數**

```bash
* 常數寫在最開頭 慣使用大寫 其實VFP不分大小寫

# DEFINE PI 3.14
# DEFINE INCOMESTATEMENT "綜合資產負債表"

=MESSAGEBox(PI)
=MESSAGEBox(INCOMESTATEMENT)

* 常數不可改變其內容

PI = 0   &&  這行會發生錯誤

```

**陣列**

```bash
DIMENSION Company[3]

Company[1]="宏碁" 
Company[2]="華碩" 
Company[3]="聯強"

MESSAGEBOX( "第一名 : "+Company[1] ) 
MESSAGEBOX( "第二名 : "+Company[2] ) 
MESSAGEBOX( "第三名 : "+Company[3] )

```

**表達式**

* 數值表達式
* 字串表達式
* 日期表達式
* 邏輯表達式

```bash
* 字串
Card_No = "A00001" 

* 字串
Card_Name = "John" 

* 數值 
Card_Age = 20 

* 日期
Card_BirthDay = {^2001/12/31}

* 數值 
Card_HitRate = 0.16

* 邏輯值  .T.  .F.  .NOT.
Card_isAmerican = .T. 

=MESSAGEBOX( Card_No       )
=MESSAGEBOX( Card_Name     )
=MESSAGEBOX( Card_Age      )
=MESSAGEBOX( Card_BirthDay ) && 日期顯示的格式可以設定
=MESSAGEBOX( Card_HitRate  )
=MESSAGEBOX( Card_isAmerican  )

```

**常用函數**

* 數值處理函數 
* 字串處理函數 
* 日期和時間函數 
* 資料類型轉換函數 
* 測試函數

**數值處理函數** 

```cpp
CLEAR

? INT(12.5)  && 取整數 12

? TRANSFORM(12345.789)&&

```

**字串處理函數** 

```cpp
? "編號:" + "A001" && 使用加號串起來

? SUBSTR("ABCDEFG",2,3) && 取部分字串 BCD 由第2個開始取3碼
```

**日期和時間函數** 

```cpp
CLEAR

? DATETIME() && 現在時間

? DATE() && 今天

JoinDate = DATE(2021,12,31) && 指定日期 {^2021/12/31}
? JoinDate
? JoinDate + 1 && 加一天

? YEAR( JoinDate)   && 取年 數字
? Month( JoinDate)  && 取月 數字
? Day( JoinDate)    && 取日 數字

```

**資料類型轉換函數** 

**測試函數**

**練習**

