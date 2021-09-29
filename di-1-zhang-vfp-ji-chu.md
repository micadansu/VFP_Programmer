# 第1章 VFP 基礎

**Visual FoxPro 系統**

* 工作看板
* 命令窗 
* 專案管理窗

**專案管理器**

* 創建專案
* 使用專案管理器
* 設定專案管理器
* 簡單的程式

**專案管理器分頁**

* Other 
* Code 
* Class 
* Document 
* Data 
* All

**練習一**

使用命令窗顯示資料

```bash
? "你好"

? "我好"

Clear
```

使用命令窗顯示當前路徑

```bash
? CurDir() && 顯示當前路徑

md "c:\vfp\Lesson1" && 新增資料夾

cd "c:\vfp\Lesson1" && 切換當前路徑

? CurDir()&& 顯示當前路徑

Clear
```

使用命窗執行 MessageBox\(\)

```bash
MessageBox("你好") && 訊息視窗
MessageBox("我好") &&訊息視窗
MessageBox("當前路徑是："+CurDir()) && 訊息視窗當前路徑
```

{% hint style="info" %}
然後，使用滑鼠右鍵Clear 清理**命令窗**
{% endhint %}

**練習二**

簡單的程式

* 確定已經有資料夾 **c:\vfp\Lesson1**
* 在此資料夾中，新增專案 **Project1**
* 自此專案中，新增程式檔 **Program1.prg**_**\(注意存放同一資料夾\)**_
* 寫入一段簡單的程式
* 執行\(Run\)

簡單的程式

```bash
MessageBox("你好") && 訊息視窗
MessageBox("我好") && 訊息視窗
MessageBox("他好") && 訊息視窗

cd "c:\vfp\Lesson1" && 切換當前路徑路徑必須存在

MessageBox("當前路徑是："+CurDir()) && 訊息視窗顯示當前路徑
```

{% hint style="info" %}
執行成功後，確定有存檔，由檔案總管確定 **Program1.prg** 有產生
{% endhint %}

**練習三**

對專案 Project1 加入 Free Table 檔名為 **Product.dbf** _**\(注意存放同一資料夾\)**_欄位為：

| 欄位名稱 | 類型 | 長度 |
| :--- | :--- | :--- |
| PRNO | 文字 | 10 |
| PRNA | 文字 | 20 |

加入三筆資料：

| PRNO | PRNA |
| :--- | :--- |
| 001 | 電腦 |
| 002 | 鍵盤 |
| 003 | 滑鼠 |

**練習四**

修改 Program1 在程式碼最後加入四行，RUN執行。

```bash
Close Tables && 關閉所有資料表防止垃圾干擾

Select 1 && 選擇工作區

Use "c:\vfp\Lesson1\Product.dbf" && 開啟資料表

Browse && 瀏覽

Close Tables ALL && 關閉所有資料表
```

{% hint style="success" %}
將專案編譯成執行檔 **Project1.exe** 並確定由檔案總管可以執行
{% endhint %}

\*\*\*\*

\*\*\*\*

