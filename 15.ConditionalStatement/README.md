# 條件判斷

## 什麼是條件判斷

&emsp;&emsp;根據維基百科的[條件(電腦程式語言)](<https://en.wikipedia.org/wiki/Conditional_(computer_programming)>)翻譯描述：

> 在計算機科學中，條件語句（即條件語句、條件表達式和條件構造）是程式設計語言構造，它們根據布爾運算式（稱為條件）的值執行不同的計算或操作或返回不同的值。</br>

&emsp;&emsp;程式在執行時會依照不同的邏輯判斷而有不同的資料處理方式，在[11.基礎數據類型](../11.BasicDataType/README.md)的` 文字``範例2 `有演示過一次，該方式主要是依照判斷的條件是否成立。

## `IF...ELSE...`

&emsp;&emsp;讀者使用以下程式碼建立並執行後應該會認為`輸出的文字`會是`V_V_NAME is NOT NULL`然而實際上輸出的會是`V_V_NAME is NULL`？

```SQL
CREATE OR REPLACE PROCEDURE MAIN IS
    V_V_NAME VARCHAR2(10);
BEGIN
    V_V_NAME := 'Luke';

    IF V_V_NAME <> '' THEN
        DBMS_OUTPUT.PUT_LINE('V_V_NAME is NOT NULL');
    ELSE
        DBMS_OUTPUT.PUT_LINE('V_V_NAME is NULL');
    END IF;
END;

/*
 *************************************************
 * Console Output Begins:
 * V_V_NAME is NULL
 * Console Output Ends.
 * "NORTHWIND"."MAIN"@NORTHWIND.XEPDB1 completed successfully.
 *************************************************
*/
```

&emsp;&emsp;主要原因是因為`Oracle`在最初設計時參考了語言設計：

-   `空字串`視為`沒有值`。
-   `NULL`視為`未知值`或`沒有值`的狀態。

&emsp;&emsp;既然`空字串` = `沒有值`，`沒有值` = `NULL`，那麼所幸`空字串 = NULL`這樣的邏輯也算是簡化了一些矛盾的問題：

-   `空字串`是否應該占用儲存空間?
-   `NULL`是否要占用儲存空間?

&emsp;&emsp;就筆者的認為多一個判斷空值的方式，會導致使用的不同與程式碼的複雜度(雖然沒什麼影響)，統一為`空字串` = `NULL`其實只要用 `IS (NOT) NULL`，就可以完整的表達邏輯判斷的部分了。就這點而言比者確實滿喜歡這樣的定義，也方便許多，然而該看法是否適用於每個讀者就因人而異了。

&emsp;&emsp;除了上述的單一條件之外也可以有多個條件或連續判斷，顯示的程式碼如下：

```SQL
CREATE OR REPLACE PROCEDURE MAIN IS
    V_V_NAME VARCHAR2(10);
BEGIN
    V_V_NAME := 'Mary';

    IF V_V_NAME <> 'Luke' AND V_V_NAME IS NOT NULL THEN
        DBMS_OUTPUT.PUT_LINE('Hello ' || V_V_NAME);
    ELSIF V_V_NAME = 'Luke' THEN
        DBMS_OUTPUT.PUT_LINE('It''s ' || V_V_NAME);
    ELSE
        DBMS_OUTPUT.PUT_LINE('V_V_NAME is NULL');
    END IF;
END;


/*
 *************************************************
 * Console Output Begins:
 * Hello Mary
 * Console Output Ends.
 * "NORTHWIND"."MAIN"@NORTHWIND.XEPDB1 completed successfully.
 *************************************************
*/
```

然而一旦條件寫多了會變得非常冗長，連續的`IF..ELSIF`數量一旦多起來要額外添加新的判斷條件會變得非常困難，這時候就要用另一種條件判斷方式`CASE...WHEN`。

## `CASE...WHEN...`

`CASE...WHEN...`主要的用意是針對單一數值進行多個對應數值的判斷，當然也可以用來作為多個條件判斷，以下會分別介紹用法：

### 用法 1

```SQL
CREATE OR REPLACE PROCEDURE MAIN IS
    V_N_SCORE NUMBER;
BEGIN
    V_N_SCORE := 70;
    CASE V_N_SCORE
        WHEN 100 THEN
            DBMS_OUTPUT.PUT_LINE('Perfect!');
        WHEN 90 THEN
            DBMS_OUTPUT.PUT_LINE('Great!');
        WHEN 80 THEN
            DBMS_OUTPUT.PUT_LINE('Good!');
        WHEN 70 THEN
            DBMS_OUTPUT.PUT_LINE('Nice!');
        WHEN 60 THEN
            DBMS_OUTPUT.PUT_LINE('Normal!');
        ELSE
            DBMS_OUTPUT.PUT_LINE('Fail!');
        END CASE;
END;

/*
 *************************************************
 * Console Output Begins:
 * Nice!
 * Console Output Ends.
 * "NORTHWIND"."MAIN"@NORTHWIND.XEPDB1 completed successfully.
 *************************************************
*/
```

### 用法 2

```SQL
CREATE OR REPLACE PROCEDURE MAIN IS
    V_N_SCORE NUMBER;
BEGIN
    V_N_SCORE := 60;
    CASE
        WHEN V_N_SCORE = 100 THEN
            DBMS_OUTPUT.PUT_LINE('Perfect!');
        WHEN V_N_SCORE >= 90 THEN
            DBMS_OUTPUT.PUT_LINE('Great!');
        WHEN V_N_SCORE >= 80 THEN
            DBMS_OUTPUT.PUT_LINE('Good!');
        WHEN V_N_SCORE >= 70 THEN
            DBMS_OUTPUT.PUT_LINE('Nice!');
        WHEN V_N_SCORE >= 60 THEN
            DBMS_OUTPUT.PUT_LINE('Normal!');
        ELSE
            DBMS_OUTPUT.PUT_LINE('Fail!');
        END CASE;
END;

/*
 *************************************************
 * Console Output Begins:
 * Normal!
 * Console Output Ends.
 * "NORTHWIND"."MAIN"@NORTHWIND.XEPDB1 completed successfully.
 *************************************************
*/
```
