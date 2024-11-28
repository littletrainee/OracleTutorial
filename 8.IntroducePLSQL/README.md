# 介紹`PL/SQL`

&emsp;&emsp;在介紹`PL/SQL`之前要先來介紹`SQL`

## 基於`SQL`的擴展-`PL/SQL`

&emsp;&emsp;`SQL`強大的地方在於可用在`關聯式資料庫管理系統（RDBMS）`，或在`關係流資料管理系統（RDSMS）`中進行流處理。</br>
&emsp;&emsp;然而在需要針對資料進行較複雜的演算法處理，或是資料筆數過於龐大使用[資料庫交易](https://zh.wikipedia.org/zh-tw/%E6%95%B0%E6%8D%AE%E5%BA%93%E4%BA%8B%E5%8A%A1)時，
會發生較高的延遲等待、敏感資訊洩漏或是無法進行演算法處理。</br>
&emsp;&emsp;基於上述的問題，`Oracle`在`1988`年開發了 **`PL/SQL`**，用於彌補`SQL`本身的不足之處。其本身語法是基於[**`ADA`**](https://zh.wikipedia.org/wiki/Ada)進行開發，
因此程式碼內處處可以看到[**`Pascal`**](https://zh.wikipedia.org/wiki/Pascal%E8%AA%9E%E8%A8%80)的影子。</br>
