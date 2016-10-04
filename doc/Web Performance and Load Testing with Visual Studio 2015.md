
#使用Visual Studio 2015 及 Test Controller & Test Agent 進行負載壓力測試

## 壓測系統主機建置

* Step 1. 壓測主機規劃:

  ![image](https://cloud.githubusercontent.com/assets/14270012/19079852/9fcdd73e-8a87-11e6-8c92-f2ec867f97b2.png)

  * 測試端主機: 
    * 先安裝 Visual Studio Enterprise 2015，再更新到 Visual Studio Enterprise 2015 upadte 3 版本。
        * [Visual Studio 2015 update 3](https://go.microsoft.com/fwlink/?LinkId=691129)
  * 測試控制器主機(Test Controller)
    * 安裝SQL Server 2014 Express 與 Visual Studio Test Controller 2013，資料庫與Test Controller 需安裝在一起。
 
  * 測試代理器主機(Test Agent)
    * 安裝Visula Studio Test Agent 2013，可安裝多台測試代理主機，以利大規模壓測。

* Step 2. 測試控制器主機安裝&設定
  
  * Step 2.1: 安裝Test Controller & 建立帳號
    * 安裝 Visual Studio Test Controller 2013，[下載點](https://www.microsoft.com/zh-tw/download/details.aspx?id=40750) 
    
    * 安裝完Test Controller後，在"本機使用者與群組"會發現它已經自動建立三個TeamTest-開頭的群組。
      ![image](https://cloud.githubusercontent.com/assets/14270012/19080500/e22ea408-8a89-11e6-95b8-989088fdc09b.png)
    
    * 新增一組壓測帳號(loadtester)，再把此帳號各別加入三個TeamTest-開頭的群組(為避免權限問題，建議把壓測帳號也加入administrators群組)。
      ![image](https://cloud.githubusercontent.com/assets/14270012/19080505/e3fcd6ba-8a89-11e6-8b86-8c11eded978e.png)
  
  * Step 2.2: 安裝&設定SQL Express
    * 下載&安裝 SQL Server 2014 Express，[下載點](https://www.microsoft.com/zh-tw/download/details.aspx?id=42299)
        * 另外可安裝SQL Server Management Studio(SSMS)查詢壓測資料被儲存至的資料庫，[下載點](https://msdn.microsoft.com/zh-  tw/library/mt238290.aspx)
    
    * 安裝完SQL Express後， 開啟SQL Server 組態管理員，啟動所有的TCP/IP通訊協定。
           
    * 開啟Windows Server的"服務"功能，啟動"SQL Server Browser"服務(若需要的話)。
      ![image](https://cloud.githubusercontent.com/assets/14270012/19080509/e6742916-8a89-11e6-9dc4-2ecfbaa1d094.png)
      
    * 確認防火牆設定，一般在安裝完Test Controller後，防火牆的port 6901後自動開啟。
      ![image](https://cloud.githubusercontent.com/assets/14270012/19080514/e96f76fc-8a89-11e6-9a75-549c5daaa613.png)
      
      (但為了避免防火牆因素造成無法正常使用，若是在區網下，建議可以先將防火牆關閉)
   
  * Step 2.3: 設定Test Controller
    * 於登入帳戶，勾選"這個帳戶"，填入Step 2.1建立的壓測帳號&密碼。
    
    * 於負載測試，勾選"設定測試控制器已進行負載測試"，在建立負載測試結果資料庫欄位填入:"hostname/SQLExpress"。
      ![image](https://cloud.githubusercontent.com/assets/14270012/19080519/edd70002-8a89-11e6-8401-df40389c1f98.png)
      
      * hostname為主機名稱，另外因為我們安裝的SQL Express版本，所以instanceu就得填SQLExpress，其他版本的SQL就沒有此限制。
      ![image](https://cloud.githubusercontent.com/assets/14270012/19080523/ef34f95e-8a89-11e6-8ded-dc25ef3c279f.png)
      
 * Step 3. 測試代理主機安裝&設定
 
   * Step 3.1: 安裝Test Agent & 建立帳號 
      * 安裝 Visual Studio Test Agent 2013，[下載點](https://www.microsoft.com/zh-tw/download/details.aspx?id=40750)
      
      * 新增一組壓測帳號(loadtester)，為避免權限問題，建議把壓測帳號加入administrators群組。
        ![image](https://cloud.githubusercontent.com/assets/14270012/19080526/f0f2d324-8a89-11e6-9d8f-7193e25867f1.png)
   
   * Step 3.2: 設定Test Agent   
      * 於登入帳戶，勾選"這個帳戶"，填入Step 3.1建立的壓測帳號&密碼後，隨即套用設定。
        ![image](https://cloud.githubusercontent.com/assets/14270012/19081063/8fbd378c-8a8b-11e6-84fb-0bbef0ccb2bf.png)
       
       (此時不能一同勾選"向TestController註冊"，這樣網路模擬驅動程式才會安裝成功，否則會出現無法註冊的錯誤)。
       ![image](https://cloud.githubusercontent.com/assets/14270012/19080711/7fae9256-8a8a-11e6-89ce-93e9c8bc8616.png)
      * 若此時是以administrator身分登入此主機，因Test Agent要以登入帳戶的"這個帳號"執行，所以要先登出主機後再以壓測帳號(loadtester)登入。
      
      * 登入並重啟Test Agent，勾選"向TestController註冊"，於測試代理程式欄位填入: "測試控制器主機名稱:6901"。
        ![image](https://cloud.githubusercontent.com/assets/14270012/19080713/80b8eb24-8a8a-11e6-9918-a43710ee4490.png)
        
      * 確認防火牆設定，一般在安裝完Test Controller後，防火牆的port 6901後自動開啟。
        ![image](https://cloud.githubusercontent.com/assets/14270012/19080718/8328f264-8a8a-11e6-86b9-1643f6fa192e.png)
      
        (但為了避免防火牆因素造成無法正常使用，若是在區網下，建議可以先將防火牆關閉)   
        
## 使用Visual Studio 2015壓測:
   
   * 建議參考: [如何進行網站壓力測試：以不動產交易實價查詢服務網為例](http://blog.miniasp.com/post/2012/10/18/How-to-do-Web-Load-Test.aspx)
   
   * 因為Visual Studio 2015後，新增了Visual Studio Online(VSO)功能，所以壓測時預設都會使用VSO的功能來執行(須連上internet)，因此在區網下
     壓測就會失敗，因此需要在已建立之專案上的"Local.testsettings" ，於"測試回合設置"，勾選"使用本機電腦或測試控制器執行測試"。
     
     ![image](https://cloud.githubusercontent.com/assets/14270012/19080724/856bd80c-8a8a-11e6-873c-98e52c7398b4.png)
   
   * 若要紀錄更詳細的壓測數據，建議在已建立專案的"負載測試"項目，於"回合設定"上，滑鼠右鍵選擇"屬性"，找到"結果">"計時詳細資料儲存區"，選擇"所有個別
     細節"後儲存設定。  
 
     ![image](https://cloud.githubusercontent.com/assets/14270012/19080725/869aac62-8a8a-11e6-94d9-e8fa66a4efd9.png)
     
      
## 補充:

   * 從Visual Studio 2015開始，因新增了Visual Studio Online(VSO)功能並取代Test Controller角色(須連上internet)，就不在需要另外建置Test 
     Controller，僅需建置Test Agent即可，但對於在區網壓測(無法連到internet)而言，建議另外安裝 Visual Studio Test Controller 2013 與 
     Visual Studio Test Agent 2013。


## Reference:
   * [如何在 Visual Studio 中儲存和開啟 Web 效能和負載測試結果](https://msdn.microsoft.com/zh-tw/library/ms404662.aspx)
   * [使用 Visual Studio 設定 Test Controller 和 Test Agent 以便管理測試](https://msdn.microsoft.com/zh-tw/library/hh546459.aspx)
   * [用 application insight 作為壓力測試目標的效能收集](https://www.visualstudio.com/zh-tw/docs/test/performance-testing/getting-started/get-performance-data-for-load-tests)
   * [使用 Visual Studio 2010 Ultimate 及 Agent 進行負載壓力測試](http://blog.miniasp.com/post/2011/03/13/Visual-Studio-Agent-2010-Load-and-Stress-Testing-Installation-Guide.aspx)
   * [Web Performance and Load Testing with Visual Studio 2012 – Remote Execution](http://blog.nwcadence.com/web-performance-and-load-testing-with-visual-studio-2012-remote-execution-2/)
   * [Test controller configuration tool] (http://answers.flyppdevportal.com/MVC/Post/Thread/d7232dfc-ef3a-461f-bc2a-be281fd800d3?category=vsmantest)
   
    
  
  
