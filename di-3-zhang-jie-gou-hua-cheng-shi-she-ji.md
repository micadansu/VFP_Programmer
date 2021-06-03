# 第3章 結構化程式設計

**程式檔\(PRG\)**

* 程式檔的基本概念
* 程式檔的建立和修改 
* 自定函數 
* 自定程序 
* 程式的運行

**程式的流程**

* 順序結構 
* 選擇結構 
* 循環結構

**多模組程式設計**

* 程式引用 
* 參數傳遞 
* 變數的有效範圍 

**自訂函數 FUNCTION**

```text
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
ENDFUNC

*****************
FUNCTION Sub(x,y)
****************
    MESSAGEBOX("這是減法")        

    RETURN x - y
ENDFUNC
```

**自訂程序 PROCEDURE**

```text
* 程序沒有回傳值

ShowAdd(10,2)

ShowSub(10,2)

**********************
PROCEDURE ShowAdd(x,y)
**********************
    cAns =  x + y    
    cAns = TRANSFORM(nAns)        
    MESSAGEBOX( "這是加法 我得到:" + TRANSFORM(nAns)   )    
ENDPROC

**********************
PROCEDURE ShowSub(x,y)
**********************
    cAns = TRANSFORM(   x - y    )    
    MESSAGEBOX("這是減法 我得到:"+ cAns  )    
ENDPROC
```

**IF 選判斷式**

```text
cPassWord="a123456789"

IF cPassWord <> "012345678" THEN 
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

```text
cAccno ="1111" && 科目代號
nCash =1000001 && 科目金額

IF (cAccno = "1111") AND (nCash > 1000000) && 括號幫助閱讀
    cMsg = "現金不可大於一百萬 " + CHR(13) + "您的現金" + TRANSFORM(nCash)    
    MESSAGEBOX( cMsg )     
ENDIF
```

**IF ELSE 選判斷式**

```text
nDamount = 100 && 借方合計
nCamount = 100 && 貸方合計

IF nDamount = nCamount 
  cMsg = "借貸平衡"
ELSE
  cMsg = "借貸不平"
ENDIF 
MESSAGEBOX( cMsg )
```

```text
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

```text
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

```text
nAccType = 7

DO CASE
CASE (nAccType = 1) OR (nAccType = 2) OR (nAccType = 3)
  cName="資產負債類"
CASE (nAccType = 4) OR (nAccType = 5) OR (nAccType = 6) OR (nAccType = 7)
  cName="綜合損益類"
OTHERWISE   
  cName="其他"
ENDCASE 

MESSAGEBOX( cName )
```

**FOR 迴圈**

```text
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

```text
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

**程式引用 Set Procedure to ..**.

```text
* Program1.prg

Set Procedure to Program2.prg ADDITIVE         && 引用另一支 .prg

nAns = Add(10,2)
=MESSAGEBOX("呼叫 ADD(x,y) 我得到:"+TRANSFORM(nAns)   )

nAns = Sub(10,2)
=MESSAGEBOX("呼叫 Sub(x,y) 我得到:"+TRANSFORM(nAns)   )
```

```text
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

**呼叫另一支程式  Do**

```text
* Program1.prg

MessageBox("我是第一支程式 呦")
MessageBox("我是第一支程式啊")

Do Program2.prg        && 呼叫另一支程式副檔名可省略當作呼叫程序用
```

```text
* Program2.prg

MessageBox("我是第二支程式喔")
MessageBox("我是第二支程式哈")

Return &&返回
```

**參數**

```text
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

```text
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

```text
* 參數不必放括號中

*************************
FUNCTION Add( )
*************************
LParameters x , y , z     &&  如果使用 Parameters 表示私有範圍


    RETURN x + y && 回傳給呼叫端    
ENDFUNC
```

```text
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

```text
* 傳值或傳址(傳參考) 

myFunction(var1, var2, ...)             &&  呼叫函數 參數傳值 Call By Value

myFunction(@var1, @var2, ...)           &&  呼叫函數 參數傳址 Call By Address(Reference)

DO myProcedure WITH (var1), (var2), ... &&  呼叫另一支程式 參數傳值 Call By Value

DO myProcedure WITH var1, var2, ...     && 呼叫另一支程式 參數傳址 Call By Address(Reference)
```

**變數有效範圍**

```text
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

**參數有效範圍**

```text
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

**練習**

