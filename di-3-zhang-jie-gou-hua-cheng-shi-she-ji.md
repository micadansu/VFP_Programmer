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

自訂函數

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

自訂程序

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

```text


```

**IF...ENDIF**

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

**IF...ELSE...ENDIF**

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

Do Case .. EndCase  

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
    cName = "綜合損益"    
OTHERWISE   
    cName = "其他"
ENDCASE 

MESSAGEBOX( cName )

```

```text
nAccType = 7

DO CASE
CASE (nAccType = 1) OR (nAccType = 2) OR (nAccType = 3)
  cName="資產負載類"
CASE (nAccType = 4) OR (nAccType = 5) OR (nAccType = 6) OR (nAccType = 7)
  cName="綜合損益類"
OTHERWISE   
  cName="其他"
ENDCASE 

MESSAGEBOX( cName )

```



**FOR ... ENDFOR** 

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

DO WHILE ... EDNDO

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

**練習**



