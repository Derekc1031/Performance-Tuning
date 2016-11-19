# 使用SQL Server Profiler 追蹤網頁SQL指令或Stored Procedure效能

SQL Server Profiler 「SQL 追蹤」的圖形化使用者介面，可用來監視 Database Engine 或 Analysis Services 的執行個體。 
您可以擷取每一個事件的相關資料，並將資料儲存至檔案或資料表，以供稍後分析。 例如，您可以監視生產環境，查看哪些預存程序由於執行速度過慢而影響效能。

* Step1. 啟動 SQK Server Profiler
 ![img] (https://cloud.githubusercontent.com/assets/14270012/20452845/83594d46-ae4e-11e6-929f-cacee48b2aca.jpg)
 
* Step2. 新增追蹤
 ![img] (https://cloud.githubusercontent.com/assets/14270012/20452848/bc60189a-ae4e-11e6-83c2-92d92c49e657.jpg)

* Step3. 選取事件範圍 (若只要追蹤網頁的SQL指令與Stored Procedure效能，只須選SQL:BatchCompeleted)
 ![img] (https://cloud.githubusercontent.com/assets/14270012/20452850/f22c8f94-ae4e-11e6-8041-ffb8692b5af0.jpg)
 
 ![img] (https://cloud.githubusercontent.com/assets/14270012/20452851/f7527006-ae4e-11e6-91e9-4a635f3cf3dc.jpg)
 
 
* Step4. 完成Step3，隨即開啟要追蹤的網頁， SQK Server Profiler便會自動擷取該頁面執行時會產生的SQL指令
 ![img] (https://cloud.githubusercontent.com/assets/14270012/20452857/0f06cbde-ae4f-11e6-80c4-12bf86056d89.jpg)
