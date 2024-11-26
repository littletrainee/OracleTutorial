# 介紹`PL/SQL`

&emsp;&emsp;在介紹`PL/SQL`之前要先來介紹`SQL`

## 什麼是`SQL`

&emsp;&emsp;`1970`年代初，由`IBM`研究院下屬愛曼登研究中心的`埃德加·科德`發表將`資料組成表格`的應用原則（`Codd's Relational Algebra`）。</br>
&emsp;&emsp;`1974`年，同一實驗室的`唐納德·錢柏林`和`雷蒙德·博伊斯`參考了`科德`的模型後，在研製關聯式資料庫管理系統`System R`中，開發出了一套規範語言`SEQUEL（Structured English Query Language，結構化英語查詢語言）`。</br>
&emsp;&emsp;`1976`年`11`月，`《IBM研究與開發雜誌》`上公布新版本的`SQL`（叫`SEQUEL/2`）。<br/>
&emsp;&emsp;`1979`年，`甲骨文公司(Oracle)`（當時名為關係式軟體公司）首先提供商用的`SQL`，`IBM`公司在`DB2`和`SQL/DS`資料庫系統中也實現了`SQL`。</br>
&emsp;&emsp;`同`年，由於`SEQUEL`在當時已經是其他公司的商標`IBM`為避免法律上的衝突將`SEQUEL`縮短改名為`SQL`。</br>
&emsp;&emsp;`1986`年`10`月，`美國 ANSI `採用`SQL`作為`關聯式資料庫管理系統`的`標準語言`（**ANSI X3. 135-1986**），後為國際標準化組織（`ISO`）採納為國際標準。</br>
&emsp;&emsp;`1989`年，`美國 ANSI `採納在**ANSI X3.135-1989**報告中定義的關聯式資料庫管理系統的`SQL`標準語言，稱為 **`ANSI SQL 89`**，該標準替代 **ANSI X3.135-1986**版本。該標準為下列組織所採納：</br>

1. 國際標準化組織，為 ISO 9075-1989 報告《Database Language SQL With Integrity Enhancement》
2. 美國聯邦政府，發布在《The Federal Information Processing Standard Publication(FIPS PUB)127》

&emsp;&emsp;目前，所有主要的關聯式資料庫管理系統支援某些形式的`SQL`，大部分資料庫至少遵守 **ANSI SQL89** 標準。

**ANSI SQL92** 標準在交叉連接（cross join）和內部連接之上，新增加了外部連接，並支援在 FROM 子句中寫連接表達式。支援集合的並運算、交運算。支援 Case (SQL)表達式。支援 CHECK 約束。建立臨時表。支援 cursor。支援事務隔離。

### 參考來源

[ChatGPT 4o](https://chatgpt.com/share/6728bf00-fad0-8000-8597-8ef22f466694)</br>
[Wiki SQL](https://zh.wikipedia.org/zh-tw/SQL)

## 基於`SQL`的擴展-`PL/SQL`

&emsp;&emsp;`SQL`強大的地方在於可用在`關聯式資料庫管理系統（RDBMS）`，或在`關係流資料管理系統（RDSMS）`中進行流處理。</br>
&emsp;&emsp;然而在需要針對資料進行較複雜的演算法處理，或是資料筆數過於龐大使用[資料庫交易](https://zh.wikipedia.org/zh-tw/%E6%95%B0%E6%8D%AE%E5%BA%93%E4%BA%8B%E5%8A%A1)時，
會發生較高的延遲等待、敏感資訊洩漏或是無法進行演算法處理。</br>
&emsp;&emsp;基於上述的問題，`Oracle`在`1988`年開發了 **`PL/SQL`**，用於彌補`SQL`本身的不足之處。其本身語法是基於[**`ADA`**](https://zh.wikipedia.org/wiki/Ada)進行開發，
因此程式碼內處處可以看到[**`Pascal`**](https://zh.wikipedia.org/wiki/Pascal%E8%AA%9E%E8%A8%80)的影子。</br>
