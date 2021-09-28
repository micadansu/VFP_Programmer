# 第8章 表單\(Form\)設計

**表單的建立與執行**

* 使用表單精靈創建表單 
* 修改表單 
* 運行表單

**表單設計器**

* 啟動表單設計器 
* 資料環境 
* 加入元件 
* 設置屬性 
* 編寫程式
* 快速增加資料綁定元件

**表單元件**

* 標籤元件 
* 線條與形狀元件 
* 圖片元件 
* 計時器元件 
* 文本框和編輯框元件 
* 微調元件 
* 選項按鈕組元件 
* 複選框元件 
* 列錶框和組合框元件 
* 頁框元件 
* 容器元件 
* 表格元件 
* 命令按鈕和命令按鈕組元件 
* OLE 元件
* 超級鏈接元件

**多重表單與表單集**

* 表單的類型 
* 主從表單之間的參數傳遞 
* 表單集

**自定義類**

* 類的創建 
* 類的使用 
* 類的編輯

#### 主視窗有兩種

```text
* 多視窗程式 可依主視窗大分兩種類

* 種類一：類似 Excel 其特點為視窗肚子是一大片空白，裡面可以產生許多子視窗
* 種類二：肚子沒有一大片空白，而是裝滿控件可操作，裡面也是可以產生許多子視窗

* 要設計第一種系統，主視窗就要以 VFP 自己的主視窗作為主視窗，也就是 _Screen 物件
* 要設計第二種系統，主視窗較要以自己建立的Form將其設定為As Top Leve Form 作為主視窗
```

#### 多視窗主程式範例

```text
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

```text
* 主程畫面 QueryUnload 詢問離開意願

IF 1=MESSAGEBOX("確定離開",1+32,"詢問")
	CLEAR EVENTS && 立即結束程式 停止等待
ELSE
	NODEFAULT && 不要執行預設的 結束程式 
ENDIF 
```

```text
* 主畫面的 Init 開啟主選單

Do Menu_Main.mpr with this,.T.

*注意：
* View 的 Generail Options 設 Top-Level-Form(僅適用主視窗為As Top Leve Form)
* Menu 的 Generate 產生程式碼


```



