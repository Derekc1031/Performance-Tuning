# 使用Jmeter進行負載壓力測試(同時進行不同測試案例之壓測) 
#### 案例要求:延續之前說明["使用Jmeter進行負載壓力測試"](/doc/JmeterPerfmonTuning.md)之案例，進行一樣的設定外，加入不同測試案例進行同時壓測。
並利用[Jmeter-Property File Reader](/doc/tag-jmeter-extn-1.1.zip)外掛，將相關Jmeter參數進行變數管理與設定

### 1. 安裝與設定Jmeter-Property File Reader外掛:
* 將外掛jar檔放置JMeter所在資料夾的/lib/ext並重啟JMeter。
* 重啟後，就可在類似下方畫面的地方看到Property File Reader
