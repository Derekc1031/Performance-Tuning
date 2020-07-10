# 使用Jmeter進行分散式壓力測試:

#### 案例要求:延續之前說明["使用Jmeter進行負載壓力測試"](/doc/JmeterPerfmonTuning.md)與["使用Jmeter同時進行不同測試案例之壓力測試"](/doc/JmeterPerfmonTuning2.md)之案例，進行相關的設定外，
另外加入Master(Controller)與Slaver主機進行分散式壓力測試。

## 1.確認Master(Controller)與Slaver主機的網路是互通，建議最好在同一個網段底下。

## 2.修改相關設定:
* 在Master(Controller)與Slaver主機的JMeter所在資料夾下的/bin/jmeter.properties，修改相關remote_hosts參數:

![image](https://user-images.githubusercontent.com/14270012/87142218-cf9a3080-c2d6-11ea-9738-e3cbbf77da9c.png)

* 本案例Slaver主機IP為10.21.56.73，Master(Controller)與Slaver主機之間網路僅有開放port 80 (如果沒有特別指定，JMeter預設會使用port 1099，需特別注意防火牆是否有開通)
* 可在remote_hosts新增多台主機IP:port(x.x.x.x:port)，可同時新增多台Slaver主機(需要逗號區隔)
 
## 3.執行Slaver主機/bin/jmeter-server.bat，確定Slaver主機正常運作:

![image](https://user-images.githubusercontent.com/14270012/87143167-3b30cd80-c2d8-11ea-91d6-ee092c63b787.png)

## 4.進行分散式(遠端)壓測:

![image](https://user-images.githubusercontent.com/14270012/87143477-bb573300-c2d8-11ea-8827-cea61e5949e0.png)

## Reference:
* [JMeter Remote Test 遠端測試](http://dog0416.blogspot.com/2017/06/stress-test-jmeter-remote-test.html)
* [Apache JMeter - Remote server](http://chiehliniceday.blogspot.com/2016/06/apache-jmeter-remote-server.html)
* [JMeter 4.0 分佈式測試原理及注意事項](https://www.jianshu.com/p/b24c271a148a)
* [JMeter 遠程測試](https://www.cnblogs.com/miniren/p/6515009.html)
