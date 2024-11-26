# 迴圈

&emsp;&emsp;請先看以下程式碼

```SQL
CREATE OR REPLACE PROCEDURE MAIN IS
    TYPE ARRAY IS VARRAY(5) OF VARCHAR2(20);
    V_A_NAME ARRAY := ARRAY('Luke', 'Mary', 'John', 'George', 'Roger');
BEGIN
    DBMS_OUTPUT.PUT_LINE('1. ' || V_A_NAME(1));
    DBMS_OUTPUT.PUT_LINE('2. ' || V_A_NAME(2));
    DBMS_OUTPUT.PUT_LINE('3. ' || V_A_NAME(3));
    DBMS_OUTPUT.PUT_LINE('4. ' || V_A_NAME(4));
    DBMS_OUTPUT.PUT_LINE('5. ' || V_A_NAME(5));
END;

/*
 *************************************************
 * Console Output Begins:
 * 1. Luke
 * 2. Mary
 * 3. John
 * 4. George
 * 5. Roger
 * Console Output Ends.
 * "NORTHWIND"."MAIN"@NORTHWIND.XEPDB1 completed successfully.
 *************************************************
*/
```

&emsp;&emsp;使用陣列要獲取資料時必須要有指定序號，範例的這個陣列有`5`個，要輸出`5`個必須要重複`5`次`DBMS_OUTPUT.PUT_LINE`，倘若今天陣列的數量到達`100`個的時候是否還要重複`100`次呢?</br>
&emsp;&emsp;這邊介紹可以更加簡化的方式`迴圈`，一般迴圈分為四種`BASIC-LOOP`、`FOR-LOOP`、`WHILE-LOOP`與 SQL 自帶的`CURSOR`

## `BASIC-LOOP`

&emsp;&emsp;此種`LOOP`結束的方式是透過`迭代`的當次當中進行條件判斷，範例如下：

```SQL
CREATE OR REPLACE PROCEDURE MAIN IS
    TYPE ARRAY IS VARRAY(5) OF VARCHAR2(20);
    V_A_NAME ARRAY := ARRAY('Luke', 'Mary', 'John', 'George', 'Roger');
    V_N_INDEX NUMBER := 1;
BEGIN

    LOOP
        DBMS_OUTPUT.PUT_LINE(TO_CHAR(V_N_INDEX) || '. ' || V_A_NAME(V_N_INDEX));
        IF SUBSTR(V_A_NAME(V_N_INDEX),1,1) = 'J' THEN
            EXIT;
        ELSIF V_N_INDEX > 5 THEN
            EXIT;
        ELSE
            V_N_INDEX := V_N_INDEX + 1;
        END IF;

    END LOOP;
END;

/*
 *************************************************
 * Console Output Begins:
 * 1. Luke
 * 2. Mary
 * 3. John
 * Console Output Ends.
 * "NORTHWIND"."MAIN"@NORTHWIND.XEPDB1 completed successfully.
 *************************************************
*/
```

&emsp;&emsp;該`LOOP`的好處是可以在必須離開的時候進行判斷條件並且成立時離開此`LOOP`，但若不進行條件判斷的跳離則迴圈會變成無窮迴圈，使用上需要稍微注意一下條件判斷的部分。

## `FOR-LOOP`

&emsp;&emsp;此種`LOOP`是指定次數的方式進行迭代(通常會是用陣列的長度作為次數的基準)，範例如下：

```SQL
CREATE OR REPLACE PROCEDURE MAIN IS
    TYPE ARRAY IS VARRAY(5) OF VARCHAR2(20);
    V_A_NAME ARRAY := ARRAY('Luke', 'Mary', 'John', 'George', 'Roger');
BEGIN
    FOR V_N_INDEX IN 1..V_A_NAME.COUNT LOOP
        DBMS_OUTPUT.PUT_LINE(TO_CHAR(V_N_INDEX) || '. ' || V_A_NAME(V_N_INDEX));
    END LOOP;
END;

/*
 *************************************************
 * Console Output Begins:
 * 1. Luke
 * 2. Mary
 * 3. John
 * 4. George
 * 5. Roger
 * Console Output Ends.
 * "NORTHWIND"."MAIN"@NORTHWIND.XEPDB1 completed successfully.
 *************************************************
*/
```

&emsp;&emsp;當然也可反向迭代，範例如下：

```SQL
CREATE OR REPLACE PROCEDURE MAIN IS
    TYPE ARRAY IS VARRAY(5) OF VARCHAR2(20);
    V_A_NAME ARRAY := ARRAY('Luke', 'Mary', 'John', 'George', 'Roger');
BEGIN
    FOR V_N_INDEX IN REVERSE 1..V_A_NAME.COUNT LOOP
        DBMS_OUTPUT.PUT_LINE(TO_CHAR(V_N_INDEX) || '. ' || V_A_NAME(V_N_INDEX));
    END LOOP;
END;

/*
 *************************************************
 * Console Output Begins:
 * 5. Roger
 * 4. George
 * 3. John
 * 2. Mary
 * 1. Luke
 * Console Output Ends.
 * "NORTHWIND"."MAIN"@NORTHWIND.XEPDB1 completed successfully.
 *************************************************
*/
```

&emsp;&emsp;兩種方式差別在於`REVERSE`保留字，其他的並未有任何差別。

## `WHILE-LOOP`

&emsp;&emsp;此種`LOOP`介於`FOR-LOOP`與`BASIC-LOOP`之間，需要有明確的條件才可進行下一個迭帶，範例如下：

```SQL
CREATE OR REPLACE PROCEDURE MAIN IS
    TYPE ARRAY IS VARRAY(5) OF VARCHAR2(20);
    V_A_NAME ARRAY := ARRAY('Luke', 'Mary', 'John', 'George', 'Roger');
    V_N_INDEX NUMBER := 0;
BEGIN
    WHILE V_N_INDEX < V_A_NAME.COUNT LOOP
        V_N_INDEX := V_N_INDEX + 1;
        IF V_N_INDEX = 3 THEN
            CONTINUE;
        END IF;
        DBMS_OUTPUT.PUT_LINE(TO_CHAR(V_N_INDEX) || '. ' || V_A_NAME(V_N_INDEX));

    END LOOP;
END;

/*
 *************************************************
 * Console Output Begins:
 * 1. Luke
 * 2. Mary
 * 4. George
 * 5. Roger
 * Console Output Ends.
 * "NORTHWIND"."MAIN"@NORTHWIND.XEPDB1 completed successfully.
 *************************************************
*/
```

## `CURSOR`

&emsp;&emsp;此種`LOOP`是部分`SQL`內建的`LOOP`可以比其他`三個`有更好的` TABLE``控制能力 `，使用的範例如下：

```SQL
CREATE OR REPLACE PROCEDURE MAIN IS
    CURSOR SHIPPER_CURSOR IS
        SELECT COMPANY_NAME,
               PHONE
          FROM SHIPPERS;
    SHIPPER_ROW SHIPPER_CURSOR%ROWTYPE;
BEGIN
    OPEN SHIPPER_CURSOR;
    LOOP
        FETCH SHIPPER_CURSOR INTO SHIPPER_ROW;
        EXIT WHEN SHIPPER_CURSOR%NOTFOUND;

        DBMS_OUTPUT.PUT_LINE('Company Name:' || SHIPPER_ROW.COMPANY_NAME || ', Phone:' || SHIPPER_ROW.PHONE);
    END LOOP;
    CLOSE SHIPPER_CURSOR;
END;

/*
 *************************************************
 * Console Output Begins:
 * Company Name:Speedy Express, Phone:(503) 555-9831
 * Company Name:United Package, Phone:(503) 555-3199
 * Company Name:Federal Shipping, Phone:(503) 555-9931
 * Console Output Ends.
 * "NORTHWIND"."MAIN"@NORTHWIND.XEPDB1 completed successfully.
 *************************************************
*/
```

&emsp;&emsp;一般在執行`SQL Statements`的時候(如：`SELECT`、`INSERT INTO`、`UPDATE`與`DELETE`)，使用都是屬`CURSOR`的一環，只是這個`CURSOR`屬於隱式`CURSOR`，而此處所使用的是顯式`CURSOR`。</br>
&emsp;&emsp;`CURSOR`的背後包含了四個動作：

1. `DECLARE`：宣告`名稱`與`定義內容`。
2. `OPEN`：開啟`查詢`、`綁定變數`與`執行查詢語句`。
3. `FETCH`：提取資料表`當前迭代`的`ROW資料`。
4. `CLOSE`：`關閉`與`釋放`變數。

&emsp;&emsp;隱式游標把上述的動作自動完成了，所以一般在使用`查詢語句`時，並未有任何問題，然而使用`PL/SQL`可以手動控制這些功能，針對每筆迭代的資料進行更加詳細的控制(如：`COLUMN_A`的資料需要進行截字，`COLUMN_B`需要進行`+10%`的計算)。</br>
&emsp;&emsp;除了上面所述的四個動作之外還有四個屬性基本屬性，分別為：

-   `%ISOPEN`：檢查當前的`CURSOR`狀態是否為`開啟`狀態。
-   `%FOUND`：檢查當前的`CURSOR`是否可以`提取ROW資料`。
-   `%NOTFOUND`：檢查當前的`CURSOR`是否不可以`提取ROW資料`。
-   `%ROWCOUNT`：獲取當前已經迭代的`ROW資料`筆數。

&emsp;&emsp;上述的屬性再搭配`EXIT WHEN`或`CONTINUE WHEN`可以做到更大彈性的控制，這些部分就給予讀者行發揮了。
