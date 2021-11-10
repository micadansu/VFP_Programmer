# 第8章 表單(Form)設計

**表單的建立與執行**

* 使用表單精靈創建表單&#x20;
* 修改表單&#x20;
* 運行表單

**表單設計器**

* 啟動表單設計器&#x20;
* 資料環境&#x20;
* 加入元件&#x20;
* 設置屬性&#x20;
* 編寫程式
* 快速增加資料綁定元件

**表單元件**

* 標籤元件&#x20;
* 線條與形狀元件&#x20;
* 圖片元件&#x20;
* 計時器元件&#x20;
* 文本框和編輯框元件&#x20;
* 微調元件&#x20;
* 選項按鈕組元件&#x20;
* 複選框元件&#x20;
* 列錶框和組合框元件&#x20;
* 頁框元件&#x20;
* 容器元件&#x20;
* 表格元件&#x20;
* 命令按鈕和命令按鈕組元件&#x20;
* OLE 元件
* 超級鏈接元件

**多重表單與表單集**

* 表單的類型&#x20;
* 主從表單之間的參數傳遞&#x20;
* 表單集

**自定義類**

* 類的創建&#x20;
* 類的使用&#x20;
* 類的編輯

#### 主視窗有兩種

```
* 多視窗程式 可依主視窗大分兩種類

* 種類一：類似 Excel 其特點為視窗肚子是一大片空白，裡面可以產生許多子視窗
* 種類二：肚子沒有一大片空白，而是裝滿控件可操作，裡面也是可以產生許多子視窗

* 要設計第一種系統，主視窗就要以 VFP 自己的主視窗作為主視窗，也就是 _Screen 物件
* 要設計第二種系統，主視窗較要以自己建立的Form將其設定為As Top Leve Form 作為主視窗
```

#### 主視窗程式範例(多視窗型)

```
TRY
	IF _vfp.StartMode=0 && 如果是開發模式
		SET DEFAULT TO c:\vfp\Lesson8 
	ENDIF 

	IF _vfp.StartMode=4 && 如果是執行檔模式
		_Screen.Visible = .F. && 隱藏狐狸主窗 
	ENDIF 

	Set Classlib To MyClassLib1  && 使用物件庫 

	* 主視窗 必須最為頂層表單 ShowWindow = As Top Level Form
	Public m.oMainForm As Form	
	m.oMainForm = Newobject("Form_Main") && WindowState= 2.Maximized 最大化
	m.oMainForm.Caption="奇勝稅務系統"
	m.oMainForm.Show() && 0.多視窗(預設) 1.單視窗

	* 主工具列 必須在頂層表單中 ShowWindow = In Top Level Form
	Public m.oMainToolBar As Toolbar
	m.oMainToolBar = Newobject("ToolBar_Main")
	m.oMainToolBar.Show() && 0.多視窗(預設) 1.單視窗
	m.oMainToolBar.Dock(0) && 停靠頂邊

	* 登入畫面  必須在頂層表單中 ShowWindow = In Top Level Form
	Public m.oLoginForm As Form
	m.oLoginForm = NEWOBJECT("Form_Login") && AutoCenter=.T. 視窗自動置中
	m.oLoginForm.Caption="登入"
	m.oLoginForm.Show() && 0.多視窗(預設) 1.單視窗

	* 產品維護畫面
	PUBLIC m.oForm_Prod as Form  && 程式在按鈕中
						 
	Read EVENTS && 等待發生 Clear Events
	
 	=MESSAGEBOX("程式結束")

FINALLY
	Clear Events
	IF _vfp.StartMode=4 && 如果是執行檔模式
		_Screen.Visible = .T. && 顯示狐狸主窗 
	ENDIF 	
	Set Sysmenu To Defa
	CLEAR ALL 
	RELEASE ALL
	
ENDTRY

```

```
* 主程畫面 QueryUnload 詢問離開意願

IF 1=MESSAGEBOX("確定離開",1+32,"詢問")
	CLEAR EVENTS && 立即結束程式 停止等待
ELSE
	NODEFAULT && 不要執行預設的 結束程式 
ENDIF 
```

```
* 主畫面的 Init 開啟主選單

Do Menu_Main.mpr with this,.T.

* Menu注意事項：
* View 的 Generail Options 設 Top-Level-Form(僅適用主視窗為As Top Leve Form)
* Menu 的 Generate 產生程式碼


```

#### 主畫面上開啟產品檔維護

```
* Command1.Click 事件

IF VARTYPE(m.oForm_Prod)="O" AND !ISNULL(m.oForm_Prod) && 如果已經產生過了，不要重複產生
	m.oForm_Prod.Show(0)
	IF m.oForm_Prod.Windowstate=1  && 如果畫面現在是最小化 1.Minimized 
		m.oForm_Prod.Windowstate=0 && 恢復正常狀態 0.Normal
	ENDIF 
ELSE
	m.oForm_Prod = CREATEOBJECT("Form_Prod")
	m.oForm_Prod.Show(0)
ENDIF 	



```

#### Grid 的進階

```
// Some code

* Grid.Init

LOCAL m.oGrid as Grid,m.oColumn as Column 
m.oGrid = thisform.grid1

m.oGrid.RecordSourceType= 1
m.oGrid.RecordSource = "AAA"
m.oGrid.ColumnCount=4 && 欄位數
m.oGrid.FontSize=12
m.oGrid.FontName="微軟正黑體"
m.oGrid.GridLineColor=RGB(192,192,192)
m.oGrid.GridLineColor=RGB(0,0,0)
* 抬頭

m.oGrid.SetAll("BackColor",RGB(128,128,0),"Header")
m.oGrid.SetAll("ForeColor",RGB(255,255,0),"Header")


FOR m.nii=1 TO  m.oGrid.ColumnCount
	m.oColumn=thisform.grid1.Columns(m.nii)
			
	DO CASE
	CASE m.nii=1 			
	
		WITH m.oColumn
			.ControlSource="prno"
			.Header1.Caption="產品編號"
			.width = 100
			.inputmask="!!!!!!!!!!" && 大寫10碼 repl("!",10)
		ENDWITH 
			
	CASE m.nii=2 	
		WITH m.oColumn
			.ControlSource="prna"
			.Header1.Caption="產品名稱"
			.width = 100
			.inputmask="xxxxxxxxxx" && 大寫10碼 repl("x",10)
			
		ENDWITH 
		
	CASE m.nii=3
		WITH m.oColumn
			.ControlSource="lqty"
			.Header1.Caption="期初數量"
			.width = 100
			.inputmask="999,999,999,999,999" && 小數三位
		ENDWITH 
		
	CASE m.nii=4
		WITH m.oColumn
			.ControlSource="lamt"
			.Header1.Caption="期初金額"
			.width = 100
			.inputmask="999,999,999,999" && 整數			
		ENDWITH 		
	ENDCASE	
	
	BINDEVENT(m.oColumn.Text1, "MouseUp",Thisform,"MyMouseUp") && 
	
ENDFOR   

* Text Disable
*m.oGrid.SetAll("Enabled" ,.F.,"Column") && 會產生 Focus 回跳問題
m.oGrid.SetAll("ReadOnly",.T.,"Column")

* 隔格行變色
*LOCAL m.cStr
*m.cStr="IIF(Mod(RECNO(),2)=0,RGB(255,255,255),RGB(255,236,217))"

*m.cStr="IIF(Mod(ASCAN(this.arrno,no),2)=0,RGB(255,255,255),RGB(255,236,217))"
*m.oGrid.SetAll("DynamicBackColor",m.cStr,"Column")



m.oGrid.Refresh




```

```
// Some code


*oInplaceEdit.LostFocus

LOCAL m.oGrid as Grid
m.oGrid = Thisform.Grid1


IF VARTYPE(this.vlaue)="C"  && 如果是字串型補足空白長度
	this.vlaue = PADR(this.vlaue,FSIZE(this.cField,This.cAlias)  )
ENDIF 

IF .Not. (This.Value=m.oGrid.Columns(This.nColIndex).Text1.Value) && 如果發生修改
	DO CASE
	CASE This.nColIndex=1
		IF !EMPTY(This.Value) && 鍵值不允許空白
		
			IF KEYMATCH(This.Value) && 鍵值不可重複
				=MESSAGEBOX("鍵值不可重複"+This.Value )
			ELSE
				REPLACE prno WITH this.Value				
			ENDIF 				
		ENDIF 	
	CASE This.nColIndex=2
		REPLACE prna WITH this.Value
	CASE This.nColIndex=3
		REPLACE Lqty WITH this.Value
	CASE This.nColIndex=4
		REPLACE Lamt WITH this.Value
	ENDCASE
ENDIF 

this.Visible = .F.  
IF LASTKEY()#13 AND LASTKEY()#9 AND LASTKEY()#27 && 滿位移動
	KEYBOARD '{TAB}'	
ENDIF 

KEYBOARD '{CTRL+G}' && setfocus to grid


```

```
// Some code

*Form.KeyPress

Lparameters nKeyCode, nShiftAltCtrl


Local m.oGrid As Grid
m.oGrid = Thisform.grid1

Local m.oColumn As Column

Local m.oInplaceEdit As TextBox
m.oInplaceEdit=Thisform.oInplaceEdit

* 測試熱鍵的值
*WAIT windows TRANSFORM(nKeyCode) + "    "+TRANSFORM(nShiftAltCtrl)+"   " + TRANSFORM(Thisform.grid1.ActiveColumn) TIMEOUT 1

Do Case
Case Vartype(Thisform.ActiveControl)<>"O" && 沒有作用中之物件

Case Thisform.ActiveControl = Thisform.oInplaceEdit && 文字框物件作用中
	Do Case
	Case nKeyCode=13 And nShiftAltCtrl=0 && ENTER 輸入確定
		If m.oGrid.Columns(m.oInplaceEdit.nColIndex).ColumnOrder=m.oGrid.ColumnCount && 如果最後一欄 往下一列
			If !Eof()
				Skip
				If Eof()
					Go Bottom
				Endif
			Endif
		Endif
		Keyboard '{TAB}'

	Case nKeyCode=27 And nShiftAltCtrl=0 && ESC

		m.oInplaceEdit.Value = m.oInplaceEdit.oColumn.Text1.Value
		m.oInplaceEdit.oColumn.SetFocus
		Nodefault

	Endcase

Case Thisform.ActiveControl = m.oGrid && 格子物件作用中

	Do Case
	Case nKeyCode=27 And nShiftAltCtrl=0 && ESC
		Nodefault
		Thisform.Release && 離開

	Case nKeyCode=1 And nShiftAltCtrl=0   && HOME 首欄
		For m.nii=1 To m.oGrid.ColumnCount
			If m.oGrid.Columns(m.nii).ColumnOrder=1
				Thisform.grid1.Columns(m.nii).SetFocus
				Exit
			Endif
		Endfor
		Nodefault

	Case nKeyCode=6 And nShiftAltCtrl=0  && END 尾欄
		For m.nii=1 To m.oGrid.ColumnCount
			If m.oGrid.Columns(m.nii).ColumnOrder=m.oGrid.ColumnCount
				Thisform.grid1.Columns(m.nii).SetFocus
				Exit
			Endif
		Endfor
		Nodefault

	Case nKeyCode=7 And nShiftAltCtrl=2  && Set Focus to Grid
		Nodefault
		m.oGrid.SetFocus

	Case nShiftAltCtrl=8 And (Between(nKeyCode,0x8140,0xA0FE) Or ; && 保留給使用者自定義字元（造字區）
		Between(nKeyCode,0xA140,0xA3BF) Or ; && 標點符號、希臘字母及特殊符號，包括在0xA259-0xA261，安放了九個計量用漢字：兙兛兞兝兡兣嗧瓩糎。
		Between(nKeyCode,0xA3C0,0xA3FE) Or ; && 保留。此區沒有開放作造字區用。
		Between(nKeyCode,0xA440,0xC67E) Or ; && 常用漢字，先按筆劃再按部首排序。
		Between(nKeyCode,0xC6A1,0xC8FE) Or ; && 保留給使用者自定義字元（造字區）
		Between(nKeyCode,0xC940,0xF9D5) Or ; && 次常用漢字，亦是先按筆劃再按部首排序。
		Between(nKeyCode,0xF9D6,0xFEFE))    && 保留給使用者自定義字元（造字區）

		Nodefault

		* 作用中的 Column
		For m.nii=1 To m.oGrid.ColumnCount
			If m.oGrid.Columns(m.nii).ColumnOrder=m.oGrid.ActiveColumn
				m.oColumn=m.oGrid.Columns(m.nii)
				AddProperty(m.oInplaceEdit,"oColumn",m.oColumn)
				AddProperty(m.oInplaceEdit,"nColINdex",m.nii)
				AddProperty(m.oInplaceEdit,"cAlias",m.oGrid.RecordSource)
				AddProperty(m.oInplaceEdit,"cField",m.oColumn.ControlSource)
				AddProperty(m.oInplaceEdit,"nColumnOrder",m.oGrid.ActiveColumn)
				Exit
			Endif
		Endfor

		* 準備文字框
		With m.oInplaceEdit
			*-----------------------------------------------------文字框屬性
			.Visible = .F.
			.InputMask = m.oColumn.Text1.InputMask
			*.Alignment=Iif(Vartype(m.oColumn.text1.Value)="C", m.oColumn.Alignment,1)
			.Alignment=m.oColumn.Text1.Alignment
			.SelectOnEntry = !(Vartype(m.oColumn.Text1.Value)="C") && 數字全選
			*-----------------------------------------------------文字框定位大小及座標
			.Top     = Objtoclient(m.oColumn.Text1,1) -1
			.Left    = Objtoclient(m.oColumn.Text1,2) -1
			.Width   = Objtoclient(m.oColumn.Text1,3) -1
			.Height  = m.oColumn.Parent.RowHeight -1
		Endwith
		m.oInplaceEdit.Visible = .T.

		With m.oInplaceEdit
			IF Vartype(m.oColumn.text1.Value)="C"
				.Value=Chr(m.nKeyCode)							
				.SetFocus
				.SelStart=Len(Trim(.Text))
				.SelLength=1
			ENDIF 
		Endwith


	Case ((nKeyCode=13 Or Between(m.nKeyCode,32,126)) And nShiftAltCtrl=0) ; && 進入輸入模式 && Between(m.nKeyCode,48,57) Or Between(m.nKeyCode,65,90) Or Between(m.nKeyCode,97,122) && 英數字
		OR ((Between(m.nKeyCode,65,90) Or Between(m.nKeyCode,97,122)) And nShiftAltCtrl=1)

		* 作用中的 Column
		For m.nii=1 To m.oGrid.ColumnCount
			If m.oGrid.Columns(m.nii).ColumnOrder=m.oGrid.ActiveColumn
				m.oColumn=m.oGrid.Columns(m.nii)
				AddProperty(m.oInplaceEdit,"oColumn",m.oColumn)
				AddProperty(m.oInplaceEdit,"nColINdex",m.nii)
				AddProperty(m.oInplaceEdit,"cAlias",m.oGrid.RecordSource)
				AddProperty(m.oInplaceEdit,"cField",m.oColumn.ControlSource)
				AddProperty(m.oInplaceEdit,"nColumnOrder",m.oGrid.ActiveColumn)
				Exit
			Endif
		Endfor

		* 準備文字框
		With m.oInplaceEdit
			*-----------------------------------------------------文字框屬性
			.Visible = .F.
			.InputMask = m.oColumn.InputMask
			*.Alignment=Iif(Vartype(m.oColumn.text1.Value)="C", m.oColumn.Alignment,1)
			.Alignment=m.oColumn.Text1.Alignment
			.SelectOnEntry = !(Vartype(m.oColumn.Text1.Value)="C") && 數字全選
			*-----------------------------------------------------文字框定位大小及座標
			.Top     = Objtoclient(m.oColumn.Text1,1) -1
			.Left    = Objtoclient(m.oColumn.Text1,2) -1
			.Width   = Objtoclient(m.oColumn.Text1,3) -1
			.Height  = m.oColumn.Parent.RowHeight -1

		Endwith
		*-----------------------------------------------------
		If (nKeyCode=13)
			m.oInplaceEdit.Value = m.oColumn.Text1.Value && 修改原值
		Else
			* 直接給新值
			If (Vartype(m.oColumn.Text1.Value)="C")
				m.oInplaceEdit.Value = Chr(nKeyCode)
				Keyboard '{RIGHTARROW}'
			Else
				m.oInplaceEdit.SelectOnEntry =.T.
				m.oInplaceEdit.Value=0
				If Inlist(nKeyCode,43,45,42,47,48) && 加號減號
					Keyboard Alltrim(Chr(Lastkey()))
				Else
					Keyboard Alltrim(Str(Val(Chr(nKeyCode))))
				Endif
			Endif
		Endif
		m.oInplaceEdit.Visible = .T.
		m.oInplaceEdit.SetFocus
		Nodefault

	Endcase

Otherwise
	* 沒有攔截...
Endcase

```

```
// Some code

* Grid.AfterRowColumnChange

Lparameters nColIndex && Order
LOCAL m.oGrid m.oGrid = thisform.grid1
Local m.oColumn
找作用中的 Column 
For m.nii=1 To m.oGrid.ColumnCount 
    If m.oGrid.Columns(m.nii).ColumnOrder = nColIndex && 作用中的 Order 
        m.oColumn=m.oGrid.Columns(m.nii) && 找到作用中的Column
        Exit && 跳出迴圈
     Endif
Endfor
If m.oColumn.Text1.SelLength<>0 
    * Do Nothing 已經亮了不必做 
Else 
    Define Window Wparent At 1,1 Size 1,1 
    Activate Windows Wparent 
    Release Windows Wparent 
Endif

```

```

*Form.MyDblClick 

Thisform.Grid1.SetFocus
KeyBoard 'Enter' && 進入編修
```

####
