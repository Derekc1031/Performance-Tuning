# Windoes IIS 效能調效 

## 啟用gzip壓縮:
  * IIS 6.0
    1. 因後面需要修改到MetaBase.xml，為了在修改時不停止IIS，需先設定IIS站台，開啟"啟用直接MetaBase編輯"
    
       針對本機電腦按"右鍵"-->內容

      ![image](https://cloud.githubusercontent.com/assets/14270012/16729214/8a1cbf86-479d-11e6-9557-d5cc6cf18cea.png)

      勾選"啟用直接MetaBase編輯"
      
      ![image](https://cloud.githubusercontent.com/assets/14270012/16729256/cbf05b20-479d-11e6-8480-9fe6bc985e3c.png)
      
    2. 開啟IIS->網站->內容
     
      ![image](https://cloud.githubusercontent.com/assets/14270012/16729280/fa2bca06-479d-11e6-8dca-ac3d1efcc22d.png)
    
    3.  點選"服務"，把HTTP壓縮的兩個選項都打勾
    
      ![image](https://cloud.githubusercontent.com/assets/14270012/16729282/002e7b06-479e-11e6-9ebf-969555908307.png)
      
    4.  回到IIS->網頁服務延伸->新增網頁服務延伸
    
      ![image](https://cloud.githubusercontent.com/assets/14270012/16729287/066b539a-479e-11e6-9fb4-be4fd84924df.png)
      
      延伸名稱輸入：IIS Compression
      
      ![image](https://cloud.githubusercontent.com/assets/14270012/16729293/0f5b67c4-479e-11e6-9561-6546756bf9b2.png)
    
    5. 新增需要的檔案：C:\WINDOWS\system32\inetsrv\gzip.dll
    
      ![image](https://cloud.githubusercontent.com/assets/14270012/16729298/1701e192-479e-11e6-8515-874ed88e8d55.png)
      
      選取「設定延伸狀態成允許」
      
      ![image](https://cloud.githubusercontent.com/assets/14270012/16729301/1c9a3cee-479e-11e6-9d07-67561a5a6068.png)
      
    6. 開啟C:\Windows\System32\inetsrv\MetaBase.xml，找到下列程式碼

       * 將HcDynamicCompressionLevel="0"改為"10"

       * HcFileExtensions="htm html txt css js "

       * HcScriptFileExtensions="asp dll exe aspx"

       ps：有兩個地方要改(修改前先備份)
      
      儲存MetaBase.xml 
      
    7. 系統管理工具->服務，重新啟用HTTP SSL、IIS Admin、World Wide Web Publishing這三個服務，
    
 
  * IIS 7.0 up
    1. IIS預設沒有安裝動態內容壓縮模組，需自行安裝此模組
    2. 啟用壓縮模組


## 在IIS中將靜態檔案設定用戶端快取(Client-Side Cache)
  * IIS 6.0
    1. 選擇需要設定用戶端快取的目錄(通常是存放靜態的網頁、圖檔、CSS、Javascrip等不常變動的檔案)
    2. 開啟IIS，於選擇好的目錄上，設定[HTTP標頭]的[啟用內容的到期限制]
    
       ![http_tag](https://cloud.githubusercontent.com/assets/14270012/16729130/193e663e-479d-11e6-962c-0c4f4ac45f6d.png)

       PS:如有多個目錄要設定快取，就必須每個目錄個別設定。

  * IIS 7.0 up
    1. 只需修改網站目錄下的web.config，在<system.webServer> 區段，並加入以下設定：
      ```
      <staticContent>
          <clientCache cacheControlMaxAge="00:10:00" cacheControlMode="UseMaxAge"/>
      </staticContent>
      ```
      
        其中 cacheControlMaxAge 為 timeSpan，其格式為 d.hh:mm:ss，其中 d 是「天數」、hh 是小時、mm 是分鐘及 ss 是秒數，其中的天數是可省略的。這項設定沒有 GUI 介面可以協助你，必須手動設定。

來源參考:http://blog.miniasp.com/post/2009/04/12/How-to-setup-Client-side-Cache-in-IIS.aspx
  
  
