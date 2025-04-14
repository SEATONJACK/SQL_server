# 內容及程式碼解說

## 資料庫 table 格式

* 採用 database：SQL server

### calendar

* 用途：儲存股市休市日期

|cloumn 名稱|型別|
|---|---|
|date|date|
|day_of_stock|int|
|other|ncarchar(50)|

### stock_info

* 用途：紀錄台灣前50大公司的股市資訊

|cloumn 名稱|型別|
|---|---|
|stock_code|varchar(50)|
|name|varchar(50)|
|type|nchar(10)|
|category|nvarchar(50)|
|isTaiwan50|bit|

### stock_price_info (因作業有給定的資料 `StockTrading_TA` 故採用此名稱)

*  用途：記錄各股票的歷年各項數值 

|cloumn 名稱|型別|
|---|---|
|Date (日期)|date|
|StockCode (股票號碼)|smallint|
|Capacity (總成交股數)|int|
|Volume (成交筆數)|bigint|
|Open (開盤價)|float|
|High (最高價)|float|
|Low (最低價)|float|
|Close (收盤價)|float|
|Change (漲跌價差)|float|
|Transcation (總成交金額)|int|
|MA5|real|
|MA10|real|
|MA20|real|
|MA60|real|
|MA120|real|
|MA240|real|
|K_value|float|
|D_value|float|
|Trend|nvarchar(50)|

### Top_10_of_Taiwan_50

*  用途：記錄台灣50大股市的資訊

|cloumn 名稱|型別|
|---|---|
|TradeDate|date|
|StockCode|smallint|
|MarketType (上市/上櫃)|nvarchar(50)|
|Volume (成交筆數)|bigint|
|OpenPrice (開盤價)|float|
|HighPrice (最高價)|float|
|LowPrice (最低價)|float|
|ClosePrice (收盤價)|float|
|PriceChange (漲跌價差)|float|
|Transcation (總成交金額)|int|
|Turnover (周轉率)|real|

### tradingVolume

*  用途：紀錄 moving average 中，位於資料的前面多少百分比才可歸類於大量/極大輛/極小量/小量 股票

|cloumn 名稱|型別|
|---|---|
|compare_with|int|
|veryHigh_volume|float|
|high_volume |float|
|low_volume |float|
|veryLow_volume |float|




## python script

* [存取DB後繪製股票線](<python script/存取DB後繪製股票線.ipynb>)：讀取 database 中的資料後繪製 candle charts
* [自動排程紀錄股票變化](<python script/自動排程紀錄股票變化.ipynb>)：讀取指定股票資料，並排程來自動爬取網路上即時股市資訊
* [爬取50大股票後存入DB](<python script/爬取50大股票後存入DB.ipynb>)：爬取網路上50大股票的資料後存入 database 中
* [爬取指定股票的資料](<python script/爬取指定股票的資料.ipynb>)：利用從 `爬取50大股票後存入DB.ipynb` 中得到的股市代碼後，再度爬取美日各式數值資料

* [紀錄股市休市日期](<python script/紀錄股市休市日期.ipynb>)：記錄台灣股市某年的休市日期
  
## SQL 程式碼

* [CalculateAllMAs.sql](sql/CalculateAllMAs.sql)：計算所有股票的 moving average
* [CalculateMovingAverages.sql](sql/CalculateMovingAverages.sql)：計算特定股票的 moving  average
* [candlestick_up_down_all.sql](sql/candlestick_up_down_all.sql)：計算並分類所有股票的上漲/下跌趨勢
* [candlestick_up_down.sql](sql/candlestick_up_down.sql)：計算並分類特定股票的上漲/下跌趨勢
* [find_date_function.sql](sql/find_date_function.sql)：根據給定的起始日期、指定的天數、是否包含起始日期以及前進或後退的方向，來找出特定數量的「交易日」
* [FindKDCrossovers.sql](sql/FindKDCrossovers.sql)：根據給定的起始日期、結束日期、股票代號，找尋其 KD 值形成的 黃金/死亡 交叉
* [FindRecentCandlePattern.sql](sql/FindRecentCandlePattern.sql)：根據給定的起始日期、結束日期、股票代號，找尋日其區間內的 candle chart 裡面的 pattern
* [GetAverageClosePrice.sql](sql/GetAverageClosePrice.sql)：根據給定的起始日期、結束日期、股票代號，計算日期區間內的平均收盤價
* [GetStockPricePattern.sql](sql/GetStockPricePattern.sql)：根據給定的起始日期、結束日期、股票代號，同時根據一開始指定的 table 中的標準判斷剖個股票在過去幾天內的最高價中屬於多少百分比位數。
* [GetStockTransactions.sql](sql/GetStockTransactions.sql)：根據給定的起始日期、結束日期、股票代號，取得此段區間內特定資料的股市資訊
* [MA_Analysis.sql](sql/MA_Analysis.sql)：根據給定的起始日期、結束日期、股票代號，和2條 moving average 線，計算兩 MA 在圖表中的相對關係。
* [sp_StockMACrossover.sql](sql/sp_StockMACrossover.sql)：根據給定的起始日期、結束日期、股票代號，和2條 moving average 線，並回傳兩條線交叉的狀況 (死亡/黃金交叉)
* [sp_StockMAPercentageFilter.sql](sql/sp_StockMAPercentageFilter.sql)：為`sp_StockMACrossover.sql` 的強化版，除了兩條線交叉的狀況 (死亡/黃金交叉)，還可以指定交叉需要相差多少比例才算數。
* [Trend_Analysis.sql](sql/Trend_Analysis.sql)：根據給定的起始日期、結束日期、股票代號，依據MA 的狀況判斷股票趨勢
* [sp_CalculateTrend_Granville](sql/sp_CalculateTrend_Granville.sql)：依據葛蘭必法則判斷當前的上漲下跌趨勢
