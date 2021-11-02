# 第10章 選單(Menu)設計

**Menu概述**

* 選單的結構&#x20;
* 設計選單

**下拉式Menu**

* 選單的結構&#x20;
* 定義選單&#x20;

**快捷Menu**

* 選單的結構&#x20;
* 定義選單&#x20;

#### Menu 有兩類

```
*種類一：橫向擺置的 Menu Bar
*種類二：直向擺置的 PopUp Menu 
```

#### Menu 產生的方法

```
*種類一：手工寫碼
*種類二：使用產生器

*一般來講，固定型的選單會使用產生器製作，
*如果是劇烈動態變動的就會使用手工寫碼。因此使用產生器是主流方式。
```

#### 頂層視窗的 Menu

```
// Some code
* 頂層視窗主Menu記得必須於View ->Genearial Opetions 勾選 Top Level Form 選項
*並且於頂層表單的init 事件中 Do xxxxx.mpr with this , .T.

*頂層表單如果是多視窗記得QueryUnload 事件
if 1=messagebox("確定離開",2+32,"提示")
    Clear Event 
else
    noDefault
endif 
```

#### Menu 的細節

```
* 選項的特殊字元\<用法
* 選項的訊息
* 選項的圖示
* 選項的組合鍵
* 選項的橫隔線\-
* 選項的關閉 Skip
* 選單的唯一名稱
* 一些不常用的 Event
* 選單的顏色配置已經不適用於當前Windows系統
* 使用Set Sysmenu to Defa 恢復VFP原來選單
```

#### **練習**
