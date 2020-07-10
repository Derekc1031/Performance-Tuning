# 使用Jmeter同時進行不同測試案例之壓力測試:

#### 案例要求:延續之前說明["使用Jmeter進行負載壓力測試"](/doc/JmeterPerfmonTuning.md)之案例，進行一樣的設定外，加入不同測試案例進行同時壓測。
並利用[Jmeter-Property File Reader](/doc/tag-jmeter-extn-1.1.zip)外掛，將相關Jmeter參數進行變數管理與設定

### 1. 安裝與設定JMeter-Property File Reader外掛:
* 將外掛jar檔放置JMeter所在資料夾的/lib/ext並重啟JMeter。
* 重啟後，就可在類似下方畫面的地方看到Property File Reader

![image](https://user-images.githubusercontent.com/14270012/87137145-0c622980-c2cf-11ea-9c96-2f8787f948f2.png)

* 將每個案例中，相關須設定的共用Jmeter參數設定成變數，並存在外部檔案(本案統一寫在json檔)

![image](https://user-images.githubusercontent.com/14270012/87137385-6105a480-c2cf-11ea-8714-385fb588f817.png)

* 可依照不同壓測案例，分配各案例的模擬壓測人數(本案是模擬同時500個使用者壓測，所以所有案例user accout會等於500)

![image](https://user-images.githubusercontent.com/14270012/87137817-16d0f300-c2d0-11ea-9cba-2b67b69c1d6c.png)

### 2. 於JMeter上設定變數輸入於相關對應參數:
![image](https://user-images.githubusercontent.com/14270012/87138987-d83c3800-c2d1-11ea-8795-a1ff4823f465.png)

![image](https://user-images.githubusercontent.com/14270012/87139042-ef7b2580-c2d1-11ea-9e25-46c96f016132.png)

* 不勾選"Run Thread Groups consecutively(ie. one a time)" (否則測試案例只會依據壓測，不會同時壓測)

![image](https://user-images.githubusercontent.com/14270012/87139225-2c471c80-c2d2-11ea-9e50-570d32bce7fd.png)

### 3. 各測試案例結果統一至報表:
* 可將相關報表建置在測試計畫內(非在測試案例內)，JMeter會統一彙整所有測試案例之測試結果

![image](https://user-images.githubusercontent.com/14270012/87139703-f6eefe80-c2d2-11ea-955d-e08820e0d18f.png)

![image](https://user-images.githubusercontent.com/14270012/87139842-24d44300-c2d3-11ea-859c-547fe74f98f3.png)

## Reference
* [JMeter – Property File Reader – A custom config element](https://www.vinsguru.com/jmeter-property-file-reader-a-custom-config-element/)
* [JMeter – How To Create Reusable & Modular Test Scripts](http://www.testautomationguru.com/jmeter-modularizing-test-scripts/)
* [How To Run Multiple Thread Groups in Multiple Test Environments](https://www.vinsguru.com/jmeter-manage-test-plan/)
* [Running JMeter Samplers with Defined Percentage Probability](https://www.blazemeter.com/blog/running-jmeter-samplers-defined-percentage-probability)
