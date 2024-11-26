# 註解

## 什麼是`註解`

根據維基百科的[註解 (程式設計)](<https://zh.wikipedia.org/zh-tw/%E8%A8%BB%E8%A7%A3_(%E7%A8%8B%E5%BC%8F%E8%A8%AD%E8%A8%88)>)描述：

> &emsp;&emsp;在電腦語言中，註解是電腦語言的一個重要組成部分，用於在原始碼中解釋程式碼的功用，可以增強程式的可讀性，可維護性，或者用於在原始碼中處理不需執行的程式碼段，來除錯程式的功能執行。<br/><br/> > &emsp;&emsp;註解在隨原始碼進入預處理器或編譯器處理後會被移除，不會在目的碼中保留其相關資訊。

良好的註解可以使讀`Code`的人可以更快的了解程式碼內的功能，也可以在過些時間後回來看`Code`會更快的了解當下編寫的概念。
註解原則上只分為兩種`單行註解`與`多行註解`，以下會分別介紹。

## `單行註解`

開頭為`--`表示此行之後的文字為註解。

### 範例 1

```SQL
CREATE OR REPLACE PROCEDURE MAIN IS
    -- MESSAGE 類型 VARCHAR2 長度 15
    V_V_MESSAGE VARCHAR2(15);
BEGIN
    ...
END;
```

### 範例 2

```SQL
CREATE OR REPLACE PROCEDURE MAIN IS
    V_V_MESSAGE VARCHAR2(15); -- MESSAGE 類型 VARCHAR2 長度 15
BEGIN
    ...
END;
```

## `多行註解`

開頭與結束分別為`/*`與`*/`，在兩個範圍內的程式碼都被視為`註解`。

```SQL
CREATE OR REPLACE PROCEDURE MAIN IS
    /*
        變數名稱：MESSAGE
        類型：VARCHAR2
        長度：15
    */
    V_V_MESSAGE VARCHAR2(15);
BEGIN
    ...
END;
```

---

&emsp;&emsp;兩者使用方式因人而異，並未有標準答案，只要提高`可讀性`的註解都是有用的。**暫時用不到的程式碼也可以用註解排除**，但筆者建議能的話最好**不要留太多已經不用的程式碼註解**，雖然對於`回復`很方便，但可能因此造成 **`可讀性`下降**，**`需切記`**。
