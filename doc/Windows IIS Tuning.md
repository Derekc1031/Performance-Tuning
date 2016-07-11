# Windoes IIS 效能調效 

## 啟用gzip壓縮:
  * IIS 6.0
    1. working...

  * IIS 7.0 up
    1. IIS預設沒有安裝動態內容壓縮模組，需自行安裝此模組
    2. 啟用壓縮模組


## 在IIS中將靜態檔案設定用戶端快取(Client-Side Cache)
  * IIS 6.0
    1. 選擇需要設定用戶端快取的目錄(通常是存放靜態的網頁、圖檔、CSS、Javascrip等不常變動的檔案)
    2. 開啟IIS，於選擇好的目錄上，設定[HTTP標頭]的[啟用內容的到期限制]
    * PS:如有多個目錄要設定快取，就必須每個目錄個別設定。

  * IIS 7.0 up
    1. 只需修改網站目錄下的web.config，在<system.webServer> 區段，並加入以下設定：
      ```
      <staticContent>
          <clientCache cacheControlMaxAge="00:10:00" cacheControlMode="UseMaxAge"/>
      </staticContent>
      ```
      
        其中 cacheControlMaxAge 為 timeSpan，其格式為 d.hh:mm:ss，其中 d 是「天數」、hh 是小時、mm 是分鐘及 ss 是秒數，其中的天數是可省略的。這項設定沒有 GUI 介面可以協助你，必須手動設定。

來源參考:http://blog.miniasp.com/post/2009/04/12/How-to-setup-Client-side-Cache-in-IIS.aspx
  
  
