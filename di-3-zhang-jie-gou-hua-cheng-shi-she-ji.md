# 第3章 結構化程式設計

**程式檔\(PRG\)**

* 程式檔的基本概念
* 程式檔的建立和修改 
* 自定函數 
* 自定程序 
* 程式的運行
  * 程式的編譯
  * 呼叫另一支程式
  * 引用另一支程式上的函數
  * 設搜尋路徑

**程式的流程**

* 順序結構 
* 選擇結構 
* 循環結構

**多模組程式設計**

* 程式引用 
* 參數傳遞 
* 變數的有效範圍 

**自訂函數 FUNCTION**

```bash
*函數有回傳值

nAns = Add(10,2)
=MESSAGEBOX("呼叫 ADD(x,y) 我得到:"+TRANSFORM(nAns)   )

nAns = Sub(10,2)
=MESSAGEBOX("呼叫 Sub(x,y) 我得到:"+TRANSFORM(nAns)   )

*****************
FUNCTION Add(x,y)
****************
    MESSAGEBOX("這是加法")        

    RETURN x + y && 回傳給呼叫端    

    *Return 之後的程式沒作用
    a=1&&這行沒作用  
    b=2&&這行沒作用

ENDFUNC&&這行可略如果後面還有函數或是最後一行了

*****************
FUNCTION Sub(x,y)
****************
    MESSAGEBOX("這是減法")        

    RETURN x - y
ENDFUNC
```

**自訂程序 PROCEDURE**

```bash
* 程序沒有回傳值

ShowAdd(10,2)

ShowSub(10,2)

**********************
PROCEDURE ShowAdd(x,y)
**********************
    cAns =  x + y    
    cAns = TRANSFORM(nAns)        
    MESSAGEBOX( "這是加法 我得到:" + TRANSFORM(nAns)   )    

    Return && 之後的程式沒作用 

    
    a=1&&這行沒作用  
    b=2&&這行沒作用

ENDPROC&& End結尾的後面通常可以直接加註解不用&符號

**********************
PROCEDURE ShowSub(x,y)
**********************
    cAns = TRANSFORM(   x - y    )    
    MESSAGEBOX("這是減法 我得到:"+ cAns  )    


ENDPROC&&沒return 執行到就這一行
```

**IF 選判斷式**

```bash
cPassWord="a123456789"

IF cPassWord <> "012345678" THEN         && 可省略then 
    cMsg = "密碼錯誤"        
    MESSAGEBOX( cMsg )     
ENDIF
```

```text
nAmt = -100

IF nAmt <= 0 
    MESSAGEBOX("金額必須大於零")
ENDIF
```

```bash
cAccno ="1111" && 科目代號
nCash =1000001 && 科目金額

IF (cAccno = "1111") AND (nCash > 1000000) && 括號幫助閱讀
    cMsg = "現金不可大於一百萬 " + CHR(13) + "您的現金" + TRANSFORM(nCash)    
    MESSAGEBOX( cMsg )     
ENDIF
```

**IF ELSE 選判斷式**

```bash
nDamount = 100 && 借方合計
nCamount = 100 && 貸方合計

IF nDamount = nCamount 
  cMsg = "借貸平衡"
ELSE
  cMsg = "借貸不平"
ENDIF 
MESSAGEBOX( cMsg )
```

```bash
* 可以層層嵌套

A = 3

If (A = 1)
    Messagebox( "A is 1" )
Else
    If (A = 2)
        Messagebox( "A is 2" )
    Else
        If (A = 3)
            Messagebox( "A is 3" )
        Else
            Messagebox( "A is ?" )
        Endif
    Endif
Endif
```

**Case 判斷式**

```bash
cAccType = "7"

DO CASE
CASE cAccType = "1"
    cName = "資產"
CASE cAccType = "2"
    cName = "負債"
CASE cAccType = "3"
    cName = "權益"
CASE cAccType = "4"
    cName = "收入"
CASE cAccType = "5"
    cName = "成本"
CASE cAccType = "6"
    cName = "費用"
CASE cAccType = "7"
    cName = "營業外收支"
CASE cAccType = "8"
    cName = "其他綜合損益"    
OTHERWISE   
    cName = "其他"
ENDCASE 

MESSAGEBOX( cName )
```

```bash
nAccType = 7

DO CASE
CASE (nAccType = 1) OR (nAccType = 2) OR (nAccType = 3)
  cName="資產負債類"
  cReportName="BalanceSheet"
CASE (nAccType = 4) OR (nAccType = 5) OR (nAccType = 6) OR (nAccType = 7)
  cName="綜合損益類"
  cReportName="IncomStatement"
OTHERWISE   
  cName="其他"
  cReportName="無此報表"
ENDCASE 

MESSAGEBOX( cName )
MESSAGEBOX( cReportName )
```

**FOR 迴圈**

```bash
DIMENSION nAmt[12]
nAmt[1] = 10
nAmt[2] = 19
nAmt[3] = 25
nAmt[4] = 33
nAmt[5] = 41
nAmt[6] = 37
nAmt[7] = 42
nAmt[8] = 61
nAmt[9] = 78
nAmt[10]= 59
nAmt[11]= 88
nAmt[12]= 93

nSum = 0
FOR i = 1 TO 12
    nSum = nSum + nAmt[ i ]
ENDFOR 

MESSAGEBOX("合計金額："+TRANSFORM(nSum))
```

**WHILE 迴圈**

```bash
DIMENSION nAmt[12]
nAmt[1] = 10
nAmt[2] = 19
nAmt[3] = 25
nAmt[4] = 33
nAmt[5] = 41
nAmt[6] = 37
nAmt[7] = 42
nAmt[8] = 61
nAmt[9] = 78
nAmt[10]= 59
nAmt[11]= 88
nAmt[12]= 93

nSum = 0
i = 1
DO WHILE i <= 12 
    nSum = nSum + nAmt[ i ]
    i    = i + 1
ENDDO 

MESSAGEBOX("合計金額："+TRANSFORM(nSum))
```

**呼叫另一支程式 Do**

```bash
* Program1.prg

MessageBox("我是第一支程式 呦")
MessageBox("我是第一支程式啊")

Do Program2.prg        && 呼叫另一支程式副檔名可省略 當作呼叫程序用

aa = 1
bb = 2
cc = 3
Do Program2.prg With aa,bb,cc       && 呼叫另一支程式 副檔名可省略
```

```bash
* Program2.prg
Lparameter aa,bb,cc && 接參數名稱不必一樣， 

MessageBox("我是第二支程式喔")
MessageBox("我是第二支程式哈")

Return && 返回

 *函數沒呼叫就沒執行
function xxx()
   messagebox("必須有人呼叫喔")
endfunc
```

**搜尋徑 Set Path To**

```bash
*如果當前料夾找不到請搜尋其他資料夾，可以設很多資料夾

Set Path to c:\vfp\Lesson3 Additive

Set Path to "c:\vfp\Lesson3" Additive

Set Path to ("c:\vfp\Lesson3") Additive

cMyPath = "c:\vfp\Lesson3"
Set Path to ( cMyPath) Additive


```

**引用另一支程式上的函數 Set Procedure to ..**.

```bash
* Program1.prg

Set Procedure to Program2.prg ADDITIVE         && 引用另一支 .prg上的函數

nAns = Add(10,2)
=MESSAGEBOX("呼叫 ADD(x,y) 我得到:"+TRANSFORM(nAns)   )

nAns = Sub(10,2)
=MESSAGEBOX("呼叫 Sub(x,y) 我得到:"+TRANSFORM(nAns)   )
```

```bash
* Program2.prg

*****************
FUNCTION Add(x,y)
****************    
    MESSAGEBOX("這是加法")            
    RETURN x + y && 回傳給呼叫端    
ENDFUNC

*****************
FUNCTION Sub(x,y)
****************    
    MESSAGEBOX("這是減法")                
    RETURN x - y
ENDFUNC
```

**參數**

```bash
* 參數傳值

X = 10
Y = 2
Z = "傳值測試"
nAns = Add( X , Y , Z )
=MESSAGEBOX("呼叫 ADD(x,y,z) 我得到:"+TRANSFORM(nAns)   )

=MESSAGEBOX("看看 z 有沒有改變 :" + z   )   && 不會改變

*********************
FUNCTION Add( x , y , z )
*********************
    MESSAGEBOX("這是加法")        

    z = "亂改一通"       && 不會影響外面的 Z
    RETURN x + y && 回傳給呼叫端    
ENDFUNC
```

```bash
* 參數傳址

X = 10
Y = 2
Z = "傳值測試"
nAns = Add( X , Y , @Z )           && 如果參數前面有老鼠
=MESSAGEBOX("呼叫 ADD(x,y,z) 我得到:"+TRANSFORM(nAns)   )

=MESSAGEBOX("看看 z 有沒有改變 :" + z   )   && 有改變


*************************
FUNCTION Add( x , y , z )
*************************
    MESSAGEBOX("這是加法")        

    z = "亂改一通"       && 會影響外面的 Z

    RETURN x + y && 回傳給呼叫端    
ENDFUNC
```

```bash
* 參數不必放括號中，是另一中寫法

*************************
FUNCTION Add
*************************
LParameters x , y , z     &&  如果使用 Parameters 表示私有範圍

    z = "亂改一通"       && 會影響外面的 Z

    RETURN x + y && 回傳給呼叫端    
ENDFUNC
```

```bash
* 參數傳值與傳址

a = 10
b = 20
ExChange( a , b ) && 參數傳值 不會交換     
MESSAGEBOX( "A=" + TRANSFORM(a) ) && 不變 10
MESSAGEBOX( "B=" + TRANSFORM(b) ) && 不變 20

ExChange( @a , @b ) && 參數傳址 會交換
MESSAGEBOX( "A=" + TRANSFORM(a) ) && 變成 20
MESSAGEBOX( "B=" + TRANSFORM(b) ) && 變成 10

**********************
PROCEDURE ExChange(a,b) && 交換內容
**********************

c = a
a = b
b = c 

ENDPROC
```

```bash
* 傳值或傳址(傳參考) 

myFunction(var1, var2, ...)             &&  呼叫函數 參數傳值 Call By Value

myFunction(@var1, @var2, ...)           &&  呼叫函數 參數傳址 Call By Address(Reference)

DO myProcedure WITH (var1), (var2), ... &&  呼叫另一支程式 參數傳值 Call By Value

DO myProcedure WITH var1, var2, ...     && 呼叫另一支程式 參數傳址 Call By Address(Reference)
```

**變數有效範圍**

```bash
* 好的程式寫法 變數都用先宣告有效範圍

Public cErrorMsg         && 全域變數 有效範圍=全部程式
cErrorMsg = "錯誤訊息"

FUNCTION SayHello()
Local A,B,C    && 當地變數 有效範圍=只有本函數程序內
Private X,Y,Z  && 私有變數 有效範圍=包含之後被呼叫的函數程序
  A=1
  B=2

  X = 10
  Y = 20
  MySubProcedure() && 呼叫下層函數
  MessageBox( TRANSFORM(Z) )
ENDFUNC

Procedure MySubProcedure() 
    * c = A + B  && 錯誤 這個函數不認識 A B C 
    Z = X + Y    && 正常 變數來自 上層函數私有 
EndProc
```

Public 有效範圍

```bash
* Public 有效範圍包括:全體、全域、公用


PUBLIC X  && <========================

x="公用變數 x"
=MESSAGEBOX("主程式使用： " + x ) && 公用變數 x

=Hello()

****************
FUNCTION Hello()
****************
  =MESSAGEBOX("Hello使用：" + x ) && 公用變數 x
  =Good() 
ENDFUNC 

****************
FUNCTION Good()
****************
  =MESSAGEBOX("Good使用：" + x ) && 公用變數 x  
ENDFUNC 

```

Private 有效範圍

```bash
* Private 範例1

PUBLIC X  

x="公用變數 X"
=MESSAGEBOX("主程式使用：" + x ) && 公用變數 x

=Hello()

****************
FUNCTION Hello()
****************
PRIVATE x && <========================
  x = "Hello 私有變數 x"

  =MESSAGEBOX("Hello使用：" + x ) && Hello 私有變數 x
  =Good() 
ENDFUNC 

****************
FUNCTION Good()
****************
  =MESSAGEBOX("Good使用：" + x ) && Hello 私有變數 x
ENDFUNC 

```

```bash
* Private 範例2

PUBLIC X  

x="公用變數 X"
=MESSAGEBOX("主程式使用：" + x ) && 公用變數 x

=Hello()

****************
FUNCTION Hello()
****************

  =MESSAGEBOX("Hello使用：" + x ) && 公用變數 x
  =Good() 
ENDFUNC 

****************
FUNCTION Good()
****************
PRIVATE x && <========================
  x = "Good 私有變數 x"

  =MESSAGEBOX("Good使用：" + x ) && Good 私有變數 x
ENDFUNC 

```

Local 有效範圍

```bash
* Local 範例 1

PUBLIC X  

x="公用變數 X"
=MESSAGEBOX("主程式使用：" + x ) && 公用變數 x

=Hello()

****************
FUNCTION Hello()
****************
LOCAL x &&　<=====================
  x = "Hello的本地變數 x"
   

  =MESSAGEBOX("Hello使用：" + x ) && Hello的本地變數 x
  =Good() 
ENDFUNC 

****************
FUNCTION Good()
****************

  =MESSAGEBOX("Good使用：" + x ) && && 公用變數 x
ENDFUNC 


```

```bash
* Local 範例 2

PUBLIC X  

x="公用變數 X"
=MESSAGEBOX("主程式使用：" + x ) && 公用變數 x

=Hello()

****************
FUNCTION Hello()
****************
   
  =MESSAGEBOX("Hello使用：" + x ) && 公用變數 x
  =Good() 
ENDFUNC 

****************
FUNCTION Good()
****************
LOCAL x &&　<=====================
  x = "Good的本地變數 x"

  =MESSAGEBOX("Good使用：" + x ) && Good的本地變數 x
ENDFUNC 

```

**參數有效範圍**

```bash
* 參數兩種有效範圍

FUNCTION Add()
Lparameters x,y && 參數有效範圍 本地局部

ENDFUNC

FUNCTION Sub()
Parameters x,y && 參數有效範圍 私有

ENDFUNC

FUNCTION Div(x ,y ) && && 參數有效範圍 本地局部

ENDFUNC
```

**練習一**

```bash
* 註解：營利事業所得稅稅率計算式如下：(T = 稅額 ，P = 課稅所得額)

* 1.金額在 120,000 元以下者，稅率為免稅，稅額為零
* 2.金額在 200,000 元以下者，速算公式為 T = ( P - 120000)  x  1/2
* 3.金額超過 200,000  元者，稅率為 20 % ，速算公式為 T = P  x  0.20

請寫一支函數，TaxOfAmount(nAmt)，可以正確傳回稅額。
```

**練習二**

```bash
* 2021年（民國110年）勞工保險投保薪資分級表

* 投保薪資等級    月薪資總額（實物給付應折現金計算）    月投薪資
* 第1級          24,000元以下                        24,000元
* 第2級          24,001元至25,200元                  25,200元
* 第3級          25,201元至26,400元                  26,400元
* 第4級          26,401元至27,600元                  27,600元
* 第5級          27,601元至28,800元                  28,800元
* 第6級          28,801元至30,300元                  30,300元
* 第7級          30,301元至31,800元                  31,800元
* 第8級          31,801元至33,300元                  33,300元
* 第9級          33,301元至34,800元                  34,800元
* 第10級         34,801元至36,300元                  36,300元
* 第11級         36,301元至38,200元                  38,200元
* 第12級         38,201元至40,100元                  40,100元
* 第13級         40,101元至42,000元                  42,000元
* 第14級         42,001元至43,900元                  43,900元
* 第15級         43,901元以上45,800元                45,800元

* 請寫出函數一，LevelOfPayment(nAmt)，能輸入薪資金額並回傳投保薪資等級
* 請寫出函數二，PaymentOfInsu(nAmt)，能輸入薪資金額並回傳月投保薪資
* 請寫出函數三，只用一支函數同時到兩種結果
```

