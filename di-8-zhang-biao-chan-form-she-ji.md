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

#### Grid 的進階 1 直接編修

```
*
*


**************************************************
*-- Class:        form_grid_edit (c:\vfp\lesson8\cl81.vcx)
*-- ParentClass:  form
*-- BaseClass:    form
*-- Time Stamp:   11/11/21 08:54:11 AM
*
DEFINE CLASS form_grid_edit AS form


	DataSession = 1
	Top = 0
	Left = 0
	Height = 497
	Width = 841
	DoCreate = .T.
	Caption = "Form1"
	KeyPreview = .T.
	Name = "Form1"


	ADD OBJECT grid1 AS grid WITH ;
		FontName = "微軟正黑體", ;
		FontSize = 12, ;
		Height = 408, ;
		Left = 12, ;
		RowHeight = 23, ;
		Top = 36, ;
		Width = 540, ;
		Name = "Grid1"


	ADD OBJECT command_insert AS commandbutton WITH ;
		Top = 48, ;
		Left = 576, ;
		Height = 85, ;
		Width = 217, ;
		FontName = "微軟正黑體", ;
		FontSize = 12, ;
		Caption = "新增", ;
		Name = "Command_Insert"


	ADD OBJECT oinplaceedit AS textbox WITH ;
		FontName = "微軟正黑體", ;
		FontSize = 12, ;
		BorderStyle = 0, ;
		Height = 49, ;
		Left = 576, ;
		Top = 156, ;
		Visible = .F., ;
		Width = 145, ;
		ForeColor = RGB(255,0,0), ;
		BackColor = RGB(255,255,0), ;
		Name = "oInplaceEdit"


	ADD OBJECT command_delete AS commandbutton WITH ;
		Top = 228, ;
		Left = 576, ;
		Height = 85, ;
		Width = 217, ;
		FontName = "微軟正黑體", ;
		FontSize = 12, ;
		Caption = "刪除", ;
		PicturePosition = 1, ;
		PictureSpacing = 0, ;
		Name = "Command_Delete"


	ADD OBJECT command3 AS commandbutton WITH ;
		Top = 360, ;
		Left = 576, ;
		Height = 85, ;
		Width = 217, ;
		FontName = "微軟正黑體", ;
		FontSize = 12, ;
		Caption = "巨集", ;
		Name = "Command3"


	PROCEDURE mymousedown
		LPARAMETERS nButton, nShift, nXCoord, nYCoord


		LOCAL m.oGrid
		m.oGrid = thisform.grid1 

		Local m.oColumn
		* 找作用中的 Column
		For m.nii=1 To m.oGrid.ColumnCount
		    If m.oGrid.Columns(m.nii).ColumnOrder = m.oGrid.ActiveColumn  && 作用中的 Order
		        m.oColumn=m.oGrid.Columns(m.nii) && 找到作用中的Column
		        Exit && 跳出迴圈
		    Endif
		Endfor

		If m.oColumn.Text1.SelLength<>0  && 已經亮了
		    m.oGrid.SetFocus
		    KEYBOARD '{ENTER}' && 進入編修模式       
		Else
		    Define Window Wparent At 1,1 Size 1,1
		    Activate Windows Wparent
		    Release Windows Wparent
		Endif
	ENDPROC


	PROCEDURE builddynamiccolorarray
		LOCAL m.oGrid as Grid 
		m.oGrid=thisform.grid1

		m.oGrid.AddProperty("DynamicColorArray[1]")

		SELECT prno FROM prod ORDER BY prno INTO ARRAY Thisform.Grid1.DynamicColorArray
	ENDPROC


	PROCEDURE Unload

		USE IN prod 

		*SELECT prod
		*USE 
	ENDPROC


	PROCEDURE Load


		SET DELETED ON


		If .Not. Used("prod")

		    Create Cursor Prod(prno c(10),prna c(10),lqty N(19,3),lamt N(19))

		    Insert Into Prod Values("002","螢幕",0,0)
		    Insert Into Prod Values("003","鍵盤",0,0)
		    Insert Into Prod Values("004","滑鼠",0,0)
		    Insert Into Prod Values("005","筆電",0,0)
		    Insert Into Prod Values("006","平板",0,0)
		    Insert Into Prod Values("001","電腦",0,0)

		    Index On prno Tag prno
		    Set Order To Tag prno
		Else
		    Select Prod
		Endif


		Go  Top
	ENDPROC


	PROCEDURE KeyPress
		Lparameters nKeyCode, nShiftAltCtrl

		*WAIT windows TRANSFORM(nKeyCode)+" "+TRANSFORM(nShiftAltCtrl) TIMEOUT 1

		Local m.oGrid As Grid
		Local m.oColumn As Column
		LOCAL m.oInplaceEdit as TextBox 
		LOCAL m.nii

		m.oGrid=Thisform.grid1
		m.oInplaceEdit=Thisform.oInplaceEdit

		Do Case
		Case Vartype(Thisform.ActiveControl)<>"O" && 沒有物件作用中
		    * Do Nothing
		    
		Case Thisform.ActiveControl=m.oInplaceEdit && 在地編修方塊物件作用中   
			DO CASE
			CASE nKeyCode=13 And nShiftAltCtrl=0 && Enter 離開修改模式
				NODEFAULT 
				m.oGrid.SetFocus 

				* 找作用中的 Column 
				FOR m.nii=1 TO m.oGrid.ColumnCount
					IF m.oGrid.Columns(m.nii).ColumnOrder = m.oGrid.ActiveColumn && 作用中的 Order
						m.oColumn=m.oGrid.Columns(m.nii) && 找到作用中的Column
						EXIT && 跳出迴圈
					ENDIF 
				ENDFOR 
				IF m.oColumn.ColumnOrder = m.oGrid.ColumnCount && 是尾欄
					* 往下跳一行
					IF !EOF()
						SKIP 
						IF EOF()
							GO BOTTOM 
						ENDIF 
					ENDIF 
				ENDIF 

				KEYBOARD '{TAB}'
			Case nKeyCode=27 And nShiftAltCtrl=0 && ESC
				m.oInplaceEdit.Value = m.oInplaceEdit.oColumn.Text1.Value
				m.oInplaceEdit.oColumn.SetFocus
				Nodefault
			ENDCASE

		    
		Case Thisform.ActiveControl=m.oGrid && 格子物件作用中

			* 找作用中的 Column 
			FOR m.nii=1 TO m.oGrid.ColumnCount
				IF m.oGrid.Columns(m.nii).ColumnOrder = m.oGrid.ActiveColumn && 作用中的 Order
					m.oColumn=m.oGrid.Columns(m.nii) && 找到作用中的Column
					EXIT && 跳出迴圈
				ENDIF 
			ENDFOR 

		    Do Case
		    Case nKeyCode=27 And nShiftAltCtrl=0  && Homw 想要停靠首欄
		    	NODEFAULT 
		    	Thisform.Release 
		    
		    Case nKeyCode=1 And nShiftAltCtrl=0  && Homw 想要停靠首欄
		    	NODEFAULT
		   		FOR m.nii=1 TO m.oGrid.ColumnCount
					IF m.oGrid.Columns(m.nii).ColumnOrder = 1 && 找排在第一的 Order
						m.oColumn=m.oGrid.Columns(m.nii) && 找到作用中的Column
						EXIT && 跳出迴圈
					ENDIF 
				ENDFOR 
				m.oColumn.SetFocus 
		    	    
		    Case nKeyCode=6 And nShiftAltCtrl=0  && End 想要停靠尾欄
		    	NODEFAULT
		   		FOR m.nii=1 TO m.oGrid.ColumnCount
					IF m.oGrid.Columns(m.nii).ColumnOrder = m.oGrid.ColumnCount && 找排在最後的 Order
						m.oColumn=m.oGrid.Columns(m.nii) && 找到作用中的Column
						EXIT && 跳出迴圈
					ENDIF 
				ENDFOR 
				m.oColumn.SetFocus 
		    
		    
		    Case nKeyCode=7 And nShiftAltCtrl=2  && InplaceEdit 想要對格子物件設游標
		    	NODEFAULT
		    	m.oGrid.SetFocus
		    

		Case nShiftAltCtrl=8 And (Between(nKeyCode,0x8140,0xA0FE) Or ; && 保留給使用者自定義字元（造字區）
				Between(nKeyCode,0xA140,0xA3BF) Or ; && 標點符號、希臘字母及特殊符號，包括在0xA259-0xA261，安放了九個計量用漢字：兙兛兞兝兡兣嗧瓩糎。
				Between(nKeyCode,0xA3C0,0xA3FE) Or ; && 保留。此區沒有開放作造字區用。
				Between(nKeyCode,0xA440,0xC67E) Or ; && 常用漢字，先按筆劃再按部首排序。
				Between(nKeyCode,0xC6A1,0xC8FE) Or ; && 保留給使用者自定義字元（造字區）
				Between(nKeyCode,0xC940,0xF9D5) Or ; && 次常用漢字，亦是先按筆劃再按部首排序。
				Between(nKeyCode,0xF9D6,0xFEFE))    && 保留給使用者自定義字元（造字區）
				NODEFAULT

				* 通知 編修方塊 作用中的 Column 
				ADDPROPERTY(m.oInplaceEdit,"oColumn",m.oColumn)
				ADDPROPERTY(m.oInplaceEdit,"cField",m.oColumn.ControlSource ) && ??? 變成了 prod.Prno ???
				ADDPROPERTY(m.oInplaceEdit,"cAlias",m.oGrid.RecordSource)
				* 準備編修方塊
				With m.oInplaceEdit
					*-----------------------------------------------------文字框屬性
					.Visible = .F.
					.InputMask = m.oColumn.Text1.InputMask
					.Alignment=m.oColumn.Text1.Alignment
					.Alignment=m.oColumn.Text1.Alignment
					.SelectOnEntry = !(Vartype(m.oColumn.Text1.Value)="C") && 數字全選
					*-----------------------------------------------------文字框定位大小及座標
					.Top     = Objtoclient(m.oColumn.Text1,1)-1
					.Left    = Objtoclient(m.oColumn.Text1,2)-1
					.Width   = Objtoclient(m.oColumn.Text1,3)+2
					.Height  = Objtoclient(m.oColumn.text1,4)+2
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
			CASE ( nShiftAltCtrl=0 AND (nKeyCode=13 Or Between(nKeyCode,48,57) Or Between(nKeyCode,65,90) Or Between(nKeyCode,97,122) Or Inlist(nKeyCode,43,45,42,47,48) ) ) OR ; && Enter 數字及英文小寫大寫
				 ( nShiftAltCtrl=1 AND (nKeyCode=13 Or Between(nKeyCode,48,57) Or Between(nKeyCode,65,90) Or Between(nKeyCode,97,122) Or Inlist(nKeyCode,43,45,42,47,48) ) ) && 按住 Shif

				* 通知 編修方塊 作用中的 Column 
				ADDPROPERTY(m.oInplaceEdit,"oColumn",m.oColumn)
				ADDPROPERTY(m.oInplaceEdit,"cField",m.oColumn.ControlSource ) && ??? 變成了 prod.Prno ???
				ADDPROPERTY(m.oInplaceEdit,"cAlias",m.oGrid.RecordSource)

				* 準備編修方塊
				With m.oInplaceEdit
					*-----------------------------------------------------文字框屬性
					.Visible = .F.
					.InputMask = m.oColumn.Text1.InputMask
					.Alignment = m.oColumn.Text1.Alignment
					.SelectOnEntry = !(Vartype(m.oColumn.text1.Value)="C")
					*-----------------------------------------------------文字框定位大小及座標
					.Top     = Objtoclient(m.oColumn.text1,1)-1
					.Left    = Objtoclient(m.oColumn.text1,2)-1
					.Width   = Objtoclient(m.oColumn.text1,3)+2
					.Height  = Objtoclient(m.oColumn.text1,4)+2
				Endwith
				*-----------------------------------------------------
				If (nKeyCode=13)
					m.oInplaceEdit.Value = m.oColumn.text1.Value
				Else
					If (Vartype(m.oColumn.text1.Value)="C")
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
				IF m.oInplaceEdit.Text="新品號"
					m.oInplaceEdit.SelLength = 6
				ENDIF 
				Nodefault

		    Endcase
		Otherwise
			* 沒有攔截
		Endcase
	ENDPROC


	PROCEDURE grid1.Init
		LOCAL m.oGrid as Grid
		LOCAL m.oColumn as Column

		m.oGrid=thisform.grid1 
		m.oGrid.DeleteMark = .F.
		m.oGrid.GridLineColor = RGB(200,200,200)
		m.oGrid.RecordSourceType= 1
		m.oGrid.RecordSource="prod"
		m.oGrid.ColumnCount = 4
		m.oGrid.FontSize =12
		m.oGrid.FontName="微軟正黑體"
		LOCAL m.nii
		FOR m.nii=1 TO m.oGrid.ColumnCount
			m.oColumn = m.oGrid.Columns(m.nii)
			DO CASE
			CASE m.nii=1 && Prno
				m.oColumn.Header1.Caption="品號"
				m.oColumn.Width=100
				m.oColumn.text1.inputmask=REPLICATE("!",10)
			CASE m.nii=2 && Pna
				m.oColumn.Header1.Caption="品名"
				m.oColumn.Width=100
			CASE m.nii=3 && Lqty
				m.oColumn.Header1.Caption="期初數量"
				m.oColumn.Width=100
				m.oColumn.text1.inputmask="999,999,999,999.999"
			CASE m.nii=4 && Lamt
				m.oColumn.Header1.Caption="期初金額"
				m.oColumn.Width=100
				m.oColumn.text1.inputmask="999,999,999,999"
			ENDCASE
			BINDEVENT(m.oColumn.Text1,"MouseDown",thisform,"MyMouseDown")
		ENDFOR 
		m.oGrid.SetAll("ReadOnly",.T.,"Column") && 唯讀

		=Thisform.BuildDynamicColorArray() && 重建格行變色陣列
		m.oGrid.SetAll("DynamicBackColor","IIF(MOD(ASCAN(This.DynamicColorArray,prno),2)=0,RGB(255,255,255),RGB(200,240,200))","Column") && 格行變色
		  
	ENDPROC


	PROCEDURE command_insert.Click


		LOCATE for prno = "新品號"
		IF EOF()
			INSERT INTO prod (prno) values("新品號")
		    =Thisform.BuildDynamicColorArray() && 格行變色陣列
		ENDIF 

		LOCAL m.oGrid
		m.oGrid = thisform.grid1 
		m.oGrid.Columns(1).SetFocus
		KEYBOARD  '{ENTER}'
	ENDPROC


	PROCEDURE oinplaceedit.LostFocus
		Local m.oGrid As Grid
		m.oGrid=Thisform.Grid1

		*Wait Windows "你輸入了"+This.Value Timeout 1

		* 處理輸入結果
		Do Case
		Case This.oColumn=m.oGrid.Columns(1) && Prno

		  	This.Value=PADR(This.Value, FSIZE("prno",this.cAlias))
		    If !Empty(This.Value)
		        If .Not. (This.Value==Prod.prno) && 你修改了 Key
		            Select Count(1) From Prod Where prno = This.Value Into Array arr
		            If arr[1]<>0
		                =Messagebox("編號不可重複")
			        	IF prod.prno="新品號" && 放棄新增
		    	    		DELETE FROM prod WHERE prno = "新品號"        
		        	    ENDIF                 
		            Else
		                Replace prno With This.Value In Prod && 寫入
		            ENDIF
		        ELSE && 沒修改
		        	IF prod.prno="新品號" && 放棄新增
		        		DELETE FROM prod WHERE prno = "新品號"        
		            ENDIF 
		        Endif
		    Endif

		Case This.oColumn=m.oGrid.Columns(2) && Prna
			IF EMPTY(this.value)
				=MESSAGEBOX('品名不可空白')
			ELSE
				REPLACE prna WITH this.value
			ENDIF 


		Case This.oColumn=m.oGrid.Columns(3) && LQty
			REPLACE LQty WITH this.value

		Case This.oColumn=m.oGrid.Columns(4) && LAMt
			REPLACE LAMt WITH this.value


		Endcase

		* 最後
		This.Visible= .F.

		IF LASTKEY()#13 AND LASTKEY()#9 AND LASTKEY()#27 && 滿位移動
			KEYBOARD '{TAB}'
		ENDIF 


		*thisform.Grid1.Setfocs && 這一行有問題!!!!
		Keyboard '{CTRL+G}'
	ENDPROC


	PROCEDURE command_delete.Click
		LOCAL m.oGrid as Grid 
		m.oGrid=thisform.grid1 


		If 1=Messagebox("確定刪除?",1+32,"提示")
		    Delete
		    Skip
		    If Eof()
		        Go Bottom
		    Endif
		Endif

		m.oGrid.SetFocus
		m.oGrid.Refresh && 別忘記
	ENDPROC


	PROCEDURE command3.Click


		* 巨集範例

		MyAlias="prod"
		MyField="prna"

		=MESSAGEBOX("品名=" + &MyField )
		=MESSAGEBOX("品名=" + EVALUATE("prna") )
		=MESSAGEBOX("品名=" + EVALUATE(MyField) )
		=MESSAGEBOX("品名=" + EVALUATE("prod.prno") )
		=MESSAGEBOX("品名=" + EVALUATE("prod"+"."+"prno") )
		=MESSAGEBOX("品名=" + EVALUATE(MyAlias+"."+MyField) )


		REPLACE prno WITH "009"
		REPLACE &MyField WITH "009"
		REPLACE (MyField) WITH "009"

		cSQL="Select prno from prod into cursor MyCursor"
		EXECSCRIPT(cSQL)

		cSQL="Select <<MyField>> from <<MyAlias>> into cursor MyCursor"
		cSQL=TEXTMERGE(cSQL) && 解巨集
		EXECSCRIPT(cSQL)
	ENDPROC


ENDDEFINE
*
*-- EndDefine: form_grid_edit
**************************************************




```

####
