# 基礎數據類型

&emsp;&emsp;程式語言基本上必定會有一些基礎的數據類型，以`C`為例基本數據類型有`數值`、`字元`、`浮點數`、與`布林值`。`PL/SQL`也不例外，然而`PL/SQL`是基於`SQL`的擴充語言，因此筆者建議不要使用`TABLE`不支援的類型例如`Boolean`(`Oracle 23ai` 有支援但筆者當作沒有因為有在使用的 `Oracle` 的版本都不會到這麼新)。

以下分別說明與舉例：

## 數值

在`Oracle`內有以下幾種數值類型：

1. `NUMBER(p[,s])`:`Oracle`最常見的數值類型可以是`整數`、`小數`或`大數`，使用`10進制`存儲。`p`指的是有幾位最小是`1`最大是`38`；`s`為`小數`可有幾位，**若 s 越大則整數位越少**。若`s`省略或`指定為0`則表示此變數類型為`整數`。
2. `FLOAT`:屬於`NUMBER`的變體，有效浮點數可自定義(限制長度)，最大值可為`126位`，使用效能上略低於`BINARY_FLOAT`與`BINARY_DOUBLE`。
3. `BINARY_FLOAT`:類似其他語言的`float`遵照`IEEE754`標準，精度為`6`至`7`位。
4. `BINARY_DOUBLE`:類似其他語言的`double`同樣遵照`IEEE754`標準，經度為`15`至`16`位。

而在`PL/SQL`內有太多種數值類型，若讀者有需要可以參考`Oracle`官方的[PL/SQL Datatypes](https://docs.oracle.com/cd/B19306_01/appdev.102/b14261/datatypes.htm)。

**本教程主要以`NUMBER`作為`整數`與`浮點數`的數據類型，因此其他類型讀者想了解可以參考`Oracle`官方的[Data Types](https://docs.oracle.com/en/database/oracle/oracle-database/19/sqlrf/Data-Types.html#GUID-A3C0D836-BADB-44E5-A5D4-265BA5968483)**

### 範例 1

```SQL
CREATE OR REPLACE PROCEDURE MAIN IS
    V_N_AGE NUMBER(15) := 18;
BEGIN
    DBMS_OUTPUT.PUT_LINE(V_N_AGE);
END;

/*
 *************************************************
 * 輸出結果：
 * Console Output Begins:
 * 18
 * Console Output Ends.
 * "NORTHWIND"."MAIN"@NORTHWIND.XEPDB1 completed successfully.
 *************************************************
*/
```

### 範例 2

```SQL
CREATE OR REPLACE PROCEDURE MAIN IS
    V_N_HEIGHT NUMBER(4,1) := 179.8;
BEGIN
    DBMS_OUTPUT.PUT_LINE(V_N_HEIGHT);
END;

/*
 *************************************************
 * 輸出結果：
 * Console Output Begins:
 * 179.8
 * Console Output Ends.
 * "NORTHWIND"."MAIN"@NORTHWIND.XEPDB1 completed successfully.
 *************************************************
*/
```

## 文字

在`Oracle`內有以下幾種文字類型：

1. `VARCHAR2(s)`：依照當前`資料庫`默認字元集為底的可變長度字串，在`Oracle 12c以上`才有可擴展到`32,767字元`的設置，若未設置最多為`4,000字元`。
2. `NVARCHAR2(s)`：使用`Unicode`作為字元集的可變長度字串與`VARCHAR2`的功能類似，但主要用於`多語言環境`。
3. `LONG`：早期`Oracle`使用的大數量文字數據類型，雖然可以存長度達`2,147,483,647`，但由於每個表只能有一個`Column`為`LONG`且不能被索引，遭作與查詢功能有被受限，因此目前是屬於棄用的類型。
4. `CHAR(s)`：與`VARCHAR2`類似，但長度固定不可變動，未使用的位置會被空格代替，最多只能存`2,000字元`。
5. `NCHAR(s)`：與`NVARCHAR2`類似，同樣與`CHAR`相同，但是用於`多語言的環境`。
6. `CLOB`：與`VARCHAR2`類似，但最大長度可以達`4,294,967,296`個字元，比較適合用在長度超過`4,000`或設置擴展到`32,767字元`後扔不夠用的類型，不用指定長度。
7. `NCLOB`：與`NVARCHAR2`類似，同樣與`CLOB`相同，但用於`多語言的環境`。

&emsp;&emsp;`PL/SQL`的字串同樣可以做串連的動作，字串相連是用`||`作為串聯而非`+`，例如：

```SQL
    V_V_TEMP := 'Hello' || ', ' || 'World!'|| ';';
```

&emsp;&emsp;`PL/SQL`的字串當中也是有可能會需要有跳脫字元的動作，使用的方事就是在要跳脫的字元前面再多一個`'`，例如：

```sql
    V_V_TEMP := 'Luke''s Age is 18.';
```

**文字部分`PL/SQL`與`資料庫`並未有所不同，本教程主要以`VARCHAR2`作為`字串`與`字元`的數據類型。**</br></br>
**由於`資料庫`本身並未支持儲存`布林值`因此本教程以`CHAR`作為`布林`的數據類型，`True`為`'Y'`，`False`為`'N'`。**

### 範例 1

```SQL
CREATE OR REPLACE PROCEDURE MAIN IS
    V_V_TEMP VARCHAR2(15);
BEGIN
    V_V_TEMP := 'Hello' || ', ' || 'World!'|| ';';
    DBMS_OUTPUT.PUT_LINE(V_V_TEMP);
    V_V_TEMP := 'Luke''s Age is 18.';
    DBMS_OUTPUT.PUT_LINE(V_V_TEMP);
END;


/*
 *************************************************
 * 輸出結果：
 * Console Output Begins:
 * Hello, World!;
 * Luke's Age is 18.
 * Console Output Ends.
 * "NORTHWIND"."MAIN"@NORTHWIND.XEPDB1 completed successfully.
 *************************************************
*/
```

### 範例 2

```SQL
CREATE OR REPLACE PROCEDURE MAIN IS
    V_C_MALE CHAR(1) ;
BEGIN
    V_C_MALE := 'Y';
    IF V_C_MALE = 'Y' THEN
        DBMS_OUTPUT.PUT_LINE('Male');
    ELSE
        DBMS_OUTPUT.PUT_LINE('Female');
    END IF;
END;

/*
 *************************************************
 * 輸出結果：
 * Console Output Begins:
 * Male
 * Console Output Ends.
 * "NORTHWIND"."MAIN"@NORTHWIND.XEPDB1 completed successfully.
 *************************************************
*/
```
