# 陣列

## 什麼是陣列

&emsp;&emsp;根據維基百科的[陣列](https://zh.wikipedia.org/zh-tw/%E6%95%B0%E7%BB%84)說明：

> 在電腦科學中，陣列資料結構（英語：array data structure），簡稱陣列（英語：Array），是由相同類型的元素（element）的集合所組成的資料結構，分配一塊連續的主記憶體來儲存。

&emsp;&emsp;簡而言之，`陣列`就是`相同數據類型`的`數據集合`。只要是程式語言都必定會有數據集合，`PL/SQL`自然也不例外，`資料庫`的各個`Table`每個`Column`用於儲存相同資料類型的資料，可以理解為一個`Table`有多少的`Column`就有多少個`陣列`。

&emsp;&emsp;陣列有分為`固定長度`與`非固定長度`，以下會分別說明使用方式與寫法

## `固定長度`的陣列

&emsp;&emsp;固定長度的陣列需先宣告陣列的`類型名稱`與其定`長度`才可使用，其結構如下：

```SQL
    TYPE {陣列類型名稱} IS VARRAY({長度}) OF {元素類型};
```

&emsp;&emsp;當宣告完陣列`類型名稱`後，需將`類型名稱`作為變數的`數據類型`才可使用，結構如下：

```SQL
    {變數名稱} {陣列類型名稱} [:= {陣列類型名稱}({元素1}, {元素2}, {元素3}, ...)];
```

&emsp;&emsp;在宣告與賦值完成，想要獲取其中元素的資料要使用循序訪問才可拿到值，循序訪問的序號是從`1`開始而非部分程式語言一樣的`0`開始。一旦宣告完長度就`不可變動`，此方法適用於不可變動長度的數據集合。</br>
&emsp;&emsp;在`PL/SQL`內只用`(`與`)`作為範圍，與`C語言`家族不同，`PL/SQL`是基於`SQL`所擴展的程式語言，`[`與`]`在`Microsoft SQL Server`是用來標示`Column 的名稱`，而`{`與`}`使用的是`BEGIN`與`END`作為區塊的`開頭`與`結束`，讀者可能會需要稍微適應一下。

### 實際使用的範例如下：

```SQL
CREATE OR REPLACE PROCEDURE MAIN IS
    TYPE ARRAY IS VARRAY(5) OF NUMBER;
    V_ARR_N_ARR1 ARRAY := ARRAY(10, 20, 30, 40, 50);
BEGIN
    FOR I IN 1..V_ARR_N_ARR1.COUNT LOOP
        DBMS_OUTPUT.PUT_LINE(V_ARR_N_ARR1(I));
    END LOOP;
END;

/*
 *************************************************
 * 輸出結果：
 * Console Output Begins:
 * 10
 * 20
 * 30
 * 40
 * 50
 * Console Output Ends.
 * "NORTHWIND"."MAIN"@NORTHWIND.XEPDB1 completed successfully.
 *************************************************
*/
```

**上述的`LOOP`部分在後變得章節會說明，讀者若有需要可以先跳至該章節了解。**

## `非固定長度`的陣列

&emsp;&emsp;非固定長度的陣列在宣告陣列時只要宣告`類型名稱`**不用**指定`長度`，其結構模板如下：

```SQL
    TYPE {陣列類型名稱} IS TABLE OF {元素類型} [INDEX BY BINARY_INTEGER];
```

&emsp;&emsp;同樣的當宣告完陣列`類型名稱`後，也需要將`類型名稱`作為變數的`數據類型`才可使用。但與`固定長度`陣列不同的是，`非固定長度`的陣列是可以調整長度的，若無上述的`INDEX BY BINARY_INTEGER`，需要每次都使用下面的方法才可擴展長度：

```SQL
    {陣列變數名稱}.EXTEND;
```

&emsp;&emsp;上述的方法一次只會擴展一個元素，適合用在陣列僅需要微調長度的方式使用，相反的若要刪減長度可以使用下面的方法：

```SQL
    {陣列變數名稱}.DELETE({要刪除的位置序號});
```

&emsp;&emsp;若陣列的長度可能會需要平凡的調控可以在宣告陣列數據類型的時候添加上`INDEX BY BINARY_INTEGER`，可以委託`PL/SQL`進行`自動管理陣列長度`的`索引表`，這時候就`可以不用使用手動擴展`的方式，然而當陣列的元素`被刪掉`後，元素原本的位置會變成`空洞`，`PL/SQL`並`不具有自動移動元素位置`的功能，若要做到元素的序號是連貫的必須要`手動移動`才可。</br>
&emsp;&emsp;在`PL/SQL`當中`索引表`同樣也是一個`映射(Map)`的數據集合，除了可以用`BINARY_INTEGER`作為`索引鍵(值)`之外也可以用`VARCHAR2`作為`索引鍵(值)`。
&emsp;&emsp;以下分別為使用 **`PL/SQL`自動管理陣列長度的<u>`索引表`</u>方式**、**手動管理陣列長度的<u>`巢狀表`</u>方式**以及**若訪問陣列位置為`空洞`的`例外訊息`**、**防範可能訪問的位置為`空洞`檢查**與**手動前移調整元素位置**的範例：

### **使用`PL/SQL`自動管理陣列長度的<u>`索引表`</u>方式**

```SQL
CREATE OR REPLACE PROCEDURE MAIN IS
    TYPE ARRAY IS TABLE OF NUMBER INDEX BY BINARY_INTEGER;
    V_ARR_N_ARR1 ARRAY;
BEGIN
    V_ARR_N_ARR1 := ARRAY(10, 20, 30, 40, 50);
    FOR I IN 1..V_ARR_N_ARR1.COUNT LOOP
        DBMS_OUTPUT.PUT_LINE(V_ARR_N_ARR1(I));
    END LOOP;

    DBMS_OUTPUT.PUT_LINE('Add New Element 60');

    -- 添加新的元素，並輸出
    V_ARR_N_ARR1(V_ARR_N_ARR1.COUNT + 1) := 60;
    FOR I IN 1..V_ARR_N_ARR1.COUNT LOOP
        DBMS_OUTPUT.PUT_LINE(V_ARR_N_ARR1(I));
    END LOOP;
END;

/*
 *************************************************
 * Console Output Begins:
 * 10
 * 20
 * 30
 * 40
 * 50
 * Add New Element 60
 * 10
 * 20
 * 30
 * 40
 * 50
 * 60
 * Console Output Ends.
 * "NORTHWIND"."MAIN"@NORTHWIND.XEPDB1 completed successfully.
 *************************************************
*/
```

### **手動管理陣列長度<u>`巢狀表`</u>方式**

```SQL
CREATE OR REPLACE PROCEDURE MAIN IS
    TYPE ARRAY IS TABLE OF NUMBER;
    V_ARR_N_ARR1 ARRAY;
BEGIN
    V_ARR_N_ARR1 := ARRAY(10, 20, 30, 40, 50);
    FOR I IN 1..V_ARR_N_ARR1.COUNT LOOP
        DBMS_OUTPUT.PUT_LINE(V_ARR_N_ARR1(I));
    END LOOP;

    DBMS_OUTPUT.PUT_LINE('Add New Element 60');
    -- 對陣列擴展一個位置
    V_ARR_N_ARR1.EXTEND;
    -- 設置新的元素，並輸出
    V_ARR_N_ARR1(V_ARR_N_ARR1.COUNT) := 60;
    FOR I IN 1..V_ARR_N_ARR1.COUNT LOOP
        DBMS_OUTPUT.PUT_LINE(V_ARR_N_ARR1(I));
    END LOOP;
END;

/*
 *************************************************
 * Console Output Begins:
 * 10
 * 20
 * 30
 * 40
 * 50
 * Add New Element 60
 * 10
 * 20
 * 30
 * 40
 * 50
 * 60
 * Console Output Ends.
 * "NORTHWIND"."MAIN"@NORTHWIND.XEPDB1 completed successfully.
 *************************************************
*/
```

### **若訪問陣列位置為`空洞`的`例外訊息`**

```SQL
CREATE OR REPLACE PROCEDURE MAIN IS
    TYPE ARRAY IS TABLE OF NUMBER INDEX BY BINARY_INTEGER;
    V_ARR_N_ARR1 ARRAY;
BEGIN
    V_ARR_N_ARR1 := ARRAY(10, 20, 30, 40, 50);

    FOR I IN 1..V_ARR_N_ARR1.COUNT LOOP
        DBMS_OUTPUT.PUT_LINE(V_ARR_N_ARR1(I));
    END LOOP;

    DBMS_OUTPUT.PUT_LINE('Delete Index 3 Element 30');

    -- 移除第3個位置的元素 '30'
    V_ARR_N_ARR1.DELETE(3);

    -- 在循序訪問到第3個位置時會出錯而拋出例外
    FOR I IN 1..V_ARR_N_ARR1.COUNT LOOP
        BEGIN
            DBMS_OUTPUT.PUT_LINE(V_ARR_N_ARR1(I));
        EXCEPTION
            WHEN NO_DATA_FOUND THEN
                DBMS_OUTPUT.PUT_LINE('Index 3 is empty (NO_DATA_FOUND).');
        END;
    END LOOP;
END;


/*
 *************************************************
 * Console Output Begins:
 * 10
 * 20
 * 30
 * 40
 * 50
 * Delete Index 3 Element 30
 * 10
 * 20
 * Index 3 is empty (NO_DATA_FOUND).
 * 40
 * Console Output Ends.
 * "NORTHWIND"."MAIN"@NORTHWIND.XEPDB1 completed successfully.
 *************************************************
*/
```

### **防範可能訪問的位置為`空洞`檢查**

```SQL
CREATE OR REPLACE PROCEDURE MAIN IS
    TYPE ARRAY IS TABLE OF NUMBER;
    V_ARR_N_ARR1 ARRAY;
BEGIN
    V_ARR_N_ARR1 := ARRAY(10, 20, 30, 40, 50);
    FOR I IN 1..V_ARR_N_ARR1.COUNT LOOP
        DBMS_OUTPUT.PUT_LINE(V_ARR_N_ARR1(I));
    END LOOP;

    DBMS_OUTPUT.PUT_LINE('Delete Index 3 Element 30');
    -- 移除第3個位置的元素 '30'
    V_ARR_N_ARR1.DELETE(3);

    FOR I IN 1..V_ARR_N_ARR1.COUNT LOOP
        -- 循序訪問時檢查當前訪問的位置是否為空洞
        IF V_ARR_N_ARR1.EXISTS(I) THEN
            DBMS_OUTPUT.PUT_LINE(V_ARR_N_ARR1(I));
        ELSE
            DBMS_OUTPUT.PUT_LINE('Index '|| I || ' is empty.');
        END IF;
    END LOOP;
END;


/*
 *************************************************
 * Console Output Begins:
 * 10
 * 20
 * 30
 * 40
 * 50
 * Delete Index 3 Element 30
 * 10
 * 20
 * Index 3 is empty.
 * 40
 * Console Output Ends.
 * "NORTHWIND"."MAIN"@NORTHWIND.XEPDB1 completed successfully.
 *************************************************
*/
```

### **手動前移調整元素位置**

```SQL
CREATE OR REPLACE PROCEDURE MAIN IS
    TYPE ARRAY IS TABLE OF NUMBER;
    V_ARR_N_ARR1 ARRAY;
BEGIN
    V_ARR_N_ARR1 := ARRAY(10, 20, 30, 40, 50);
    FOR I IN 1..V_ARR_N_ARR1.COUNT LOOP
        DBMS_OUTPUT.PUT_LINE(V_ARR_N_ARR1(I));
    END LOOP;

    DBMS_OUTPUT.PUT_LINE('Delete Index 3 Element 30');

    -- 移除第3個位置的元素 '30'
    V_ARR_N_ARR1.DELETE(3);

    -- 手動前移時，原本長度會扣掉空洞，但在把空洞補上後長度會多一個
    -- 因此原本的長度要在 + 1 才可以抓到最後一個元素
    FOR I IN 4..V_ARR_N_ARR1.COUNT + 1 LOOP
        V_ARR_N_ARR1(I - 1) := V_ARR_N_ARR1(I);
    END LOOP;

    -- 刪除最後一個多餘元素
    V_ARR_N_ARR1.TRIM;

    FOR I IN 1..V_ARR_N_ARR1.COUNT LOOP
        DBMS_OUTPUT.PUT_LINE(V_ARR_N_ARR1(I));
    END LOOP;
END;

/*
 *************************************************
 * Console Output Begins:
 * 10
 * 20
 * 30
 * 40
 * 50
 * Delete Index 3 Element 30
 * 10
 * 20
 * 40
 * 50
 * Console Output Ends.
 * "NORTHWIND"."MAIN"@NORTHWIND.XEPDB1 completed successfully.
 *************************************************
*/
```
