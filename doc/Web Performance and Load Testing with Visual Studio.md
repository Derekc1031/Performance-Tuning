
#Web Performance and Load Testing with Visual Studio 2015

* Step 1. 壓測主機規劃:

  image 1

  * 測試端主機: 
    * 先安裝 Visual Studio Enterprise 2015，再更新到 Visual Studio Enterprise 2015 upadte 3 版本。
        * [Visual Studio 2015 update 3](https://go.microsoft.com/fwlink/?LinkId=691129)
  * 測試控制器主機(Test Controller)
    * 安裝SQL Server 2014 Express 與 Visual Studio Test Controller 2013，資料庫與Test Controller 需安裝在一起。
 
  * 測試代理器主機(Test Agent)
    * 安裝Visula Studio Test Agent 2013，可安裝多台測試代理主機，以利大規模壓測。

* Step 2. 測試控制器主機安裝&設定
  
  * Step 2.1: 安裝&設定Test Controller
    * 安裝 Visual Studio Test Controller 2013，[下載點](https://www.microsoft.com/zh-tw/download/details.aspx?id=40750)
    * 開啟"本機使用者與群組"，在安裝完Test Controller後，會自動建立TeamTest-開頭的群組 :
      image 2
       
    * 把壓測帳號(loadtest)加入administrators群組  
  * Step 2.2:
    * 下載&安裝 SQL Server 2014 Express，[下載點](https://www.microsoft.com/zh-tw/download/details.aspx?id=42299)
      * 另外可安裝SQL Server Management Studio(SSMS)查詢壓測資料被儲存至的資料庫，[下載點](https://msdn.microsoft.com/zh-  tw/library/mt238290.aspx)
      



##Reference:
  * 壓測環境建置&設定:
    * [如何在 Visual Studio 中儲存和開啟 Web 效能和負載測試結果](https://msdn.microsoft.com/zh-tw/library/ms404662.aspx)
    * [使用 Visual Studio 設定 Test Controller 和 Test Agent 以便管理測試](https://msdn.microsoft.com/zh-tw/library/hh546459.aspx)
    * [用 application insight 作為壓力測試目標的效能收集](https://www.visualstudio.com/zh-tw/docs/test/performance-testing/getting-started/get-performance-data-for-load-tests)
    * [使用 Visual Studio 2010 Ultimate 及 Agent 進行負載壓力測試](http://blog.miniasp.com/post/2011/03/13/Visual-Studio-Agent-2010-Load-and-Stress-Testing-Installation-Guide.aspx)
    * [Web Performance and Load Testing with Visual Studio 2012 – Remote Execution](http://blog.nwcadence.com/web-performance-and-load-testing-with-visual-studio-2012-remote-execution-2/)
    * [Test controller configuration tool] (http://answers.flyppdevportal.com/MVC/Post/Thread/d7232dfc-ef3a-461f-bc2a-be281fd800d3?category=vsmantest)
   
  
  * 如何壓測:
    * [如何進行網站壓力測試：以不動產交易實價查詢服務網為例](http://blog.miniasp.com/post/2012/10/18/How-to-do-Web-Load-Test.aspx)
  
  
