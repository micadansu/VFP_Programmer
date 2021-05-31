# 第2章 資料型態

**常數與變數**

* 註解
* 變數
* 常數
* 陣列

**註解**

```text
* 這是註解

&& 這個是註解
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

=MESSAGEBOX(PI)

=MESSAGEBOX(INCOMESTATEMENT)

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

**數值表達式**

```cpp
* 變數給值

CLEAR 
 
Card_No = "A00001"  && 字串 
Card_Name = "John"  && 字串  
Card_Age = 20       && 數值 
Card_HitRate = 0.16 && 數值  
Card_BirthDay = {^2001/12/31} && 日期 
Card_isAmerican = .T. && 邏輯值  .T.  .F.  .NOT. 

? Card_No       
? Card_Name     
? Card_Age      
? Card_BirthDay  && 日期顯示的格式可以設定
? Card_HitRate  
? Card_isAmerican  

```

**字串表達式**

```cpp
* 字串只能接字串

CLEAR 

cMemo = ""
cMemo = cMemo + "AAA" + "BBB"
cMemo = cMemo + "CCC" + "DDD"
? cMemo

cLable_No = "編號 : "
? cLable_No + Card_No

cLable_Age = "年齡 : "
* ? cLable_Age + Card_Age     && 錯誤 不能接 數值
? cLable_Age + TRANSFORM(Card_Age) && 數值變成字串就可以了


```

**數值表達式**

```cpp
* 數值的數學運算 + - * / 使用 () 先乘除後加減 

CLEAR 

nBouns = 24 * 60 * 60 && 可以 + - * /
* nBonus = 100 / (2-2) && 除到零會發生錯誤
? nBouns 
? "分數 : "+TRANSFORM(nBouns) && 轉成文字串

```

**日期表達式**

```cpp

* 日期也可運算

CLEAR 

SET CENTURY ON && 設定顯示世紀 年四碼

SET DATE YMD   && 設定顯示順序 年月日

JoinDate =  {^2001/12/31}

? JoinDate + 1   && 一天後是哪天

? JoinDate+100   && 100 天後哪天

LeaveDate = GOMONTH(JoinDate,12)  && 12 個月後

? LeaveDate

? LeaveDate-JoinDate && 相差幾天

```

邏輯表達式

```cpp
* 邏輯值運算 > = <  .And.   .Or.    .Not.  

CLEAR 

? 1 > 2 && .F.
? 1 = 2 && .F,
? 1 < 2 && .T.
? .Not. (1 = 2) && .T.
? (6 > 2) And ( 5 > 3) && .T.
? (6 > 2) Or  ( 5 < 3) && .T.


Yes = .T.  && 真 True   
No  = .F.  && 假 False 

? Yes && .T. 真
? No  && .F. 假

? .Not. Yes && .F. 非真=>假
? .Not. No  && .T. 非假=>真

? Yes = Yes   && .T. 兩邊相等 
? No  = No    && .T. 兩邊相等 
? Yes And No  && .F. 兩邊必須都.T.才是 .T. 
? Yes Or No   && .T. 只要有一邊.T.就是 .T. 

X=100
Y=20
XisBigger = (X > Y) && X比較大 ?
? XisBigger && .T. 是的

? "A002" > "A001"  && .T. 號碼
? "Boy" > "Animal" && .T. 字典
? DATE(2021,12,31) > DATE(2021,12,30) && .T. 日期
? ("A002" > "A001" ) AND (DATE(2021,12,31) > DATE(2021,12,30)) && .T.  



```

**常用函數**

* 數值函數 
* 字串函數 
* 日期和時間函數 
* 資料類型轉換函數 
* 測試變數類型

**數值函數** 

```cpp
CLEAR

? INT(12.5)  && 取整數 12

? MAX( 100 , 20 ) && 取大的數值

? MIN( 100 , 20 ) && 取小的數值


```

**字串函數** 

```cpp
CLEAR

cText="ABCDEFG"

? LEFT(cText,3)     && ABC 左三碼

? RIGHT(cText,3)    && EFG 右三碼

? SUBSTR(cText,2,3) && BCD 由第2個開始取3碼

```

**日期和時間函數** 

```cpp
CLEAR

? DATETIME() && 現在時間

? DATE() && 今天

JoinDate = DATE(2021,12,31) && 指定日期 {^2021/12/31}

? JoinDate

? JoinDate + 1 && 加一天

```

**資料類型轉換函數** 

```cpp
* 字串轉數值

CLEAR

? VAL("12")       && 數值 12

? VAL("98.21")    && 數值 98.21

```

```cpp
* 數值轉字串

CLEAR

? TRANSFORM(12)    && 字串 12

? TRANSFORM(98.21) && 字串 98.21

```

```cpp
* 邏輯轉字串

CLEAR

? TRANSFORM( .T. )

```

```cpp
* 日期轉數字

CLEAR

JoinDate = DATE(2021,12,31)

? YEAR( JoinDate)   && 取年 數值

? Month( JoinDate)  && 取月 數值

? Day( JoinDate)    && 取日 數值

? DOW( JoinDate )   && 星期幾 數值

? DAY( JoinDate )   && 該月的幾日 數值

```

```cpp
* 日期轉字串

CLEAR

JoinDate = DATE(2021,12,31)

? DTOS( JoinDate )  && 日期轉字串 20211231 格式固定

```

```cpp
* 日期格式可設定

SET CENTURY ON && 設定日期顯示世紀 年四碼

SET Date YMD   && 設定日期顯示順序 年月日

CLEAR

? DTOC( JoinDate )  && 日期轉字串 ()


```

**測試變數類型**

```cpp
* 測試格式種類

CLEAR

MyNumber = 123
MyString = "123"
MyDate   = DATE(2021,12,31) && {^2021/12/31}
MyLogic  = .F. && 假 

? VarType(MyNumber) && N
? VarType(MyString) && C
? VarType(MyDate)   && D
? VarType(MyLogic)  && L

? Type("MyNumber") && N
? Type("MyString") && C
? Type("MyDate")   && D
? Type("MyLogic")  && L

```

**練習一**

嘗試看懂這段程式，用自己的話語，默寫出解題步驟

```cpp
* 傳票號碼 下一號

*   已知傳票號碼編碼方式為：月日四碼加流水號四碼，例如一月一號第一張 "01010001"
*   現在已知 目前號碼為 "12310009" 請僅由此號碼推算下一張號碼

cBill="12310009"          && 下一張傳票應該是 "12310010"
cA=LEFT(cBill,4)          && "1231" 取左四碼月日
cB=RIGHT(cBill,4)         && "0009" 取右四碼流水號

nMyNumber = VAL(cB)       && 9  右四碼轉數字
nMyNumber = nMyNumber + 1 && 10 數字加上一

cB = TRANSFORM(nMyNumber) && "10" 數字右轉成字串
cB = "0000"+cB            && "000010" 左邊補上許多個"0" 多補沒關係但至少三個
cB = RIGHT(cB,4)          &&   "0010" 但是我們只要右邊四碼

cNewBill = cA + cB        && "12310010" = 左邊"1231"+ 右邊"0010"  
MESSAGEBOX(cNewBill)      && 答案




```

**練習**二

* 記得第一章嗎？請**複製**此程式製作 c:\vfp\Lesson2\Project2.exe 並執行成功。

**練習三**

* **白手起家**，刪除 ****c:\vfp\Lesson2 資料夾，親手憑空編寫造出此程式**。**
* 製作 c:\vfp\Lesson2\Project2.exe 並執行成功。

如果忘記，就再複習，全部刪除重來。可以要求老師再教一次\(數次\)不要客氣喔。

{% hint style="info" %}
**完全白手起家才算成功喔，加油**。
{% endhint %}

\*\*\*\*







\*\*\*\*



\*\*\*\*

