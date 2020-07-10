# 使用Jmeter進行負載壓力測試 
#### 案例要求: 證件辨識API壓測，500個使用者執行交易，每位使用者依序發出request，同一使用者間隔60~100秒，測試時間30分鐘，系統平均回應時間不得高於5秒。

#### Jmeter重點參數設定：
## 執行緒群組(Thread Group):
![image](https://user-images.githubusercontent.com/14270012/84729556-949e2900-afc6-11ea-8f06-8f0a66d7d48e.png)
* **執行緒數量(Number of Threads(users))**: 就是使用者數量，本案例為500個使用者。
* **啟動延遲(Ramp-up period)**: 多久時間內達到執行緒(使用者)數量。本案設定1500秒衝到500個使用者，若設定太短會使系統負載過重，使得結果失真，但設定太長則會使系統負載過輕，壓不出系統之瓶頸。本案例整體測試時間30分鐘(1800秒)，不建議將此設定為1800秒(整體測試時間)，因為Jmeter特性，會在接近1800秒時讓使用者數量快速下降，導致最後壓測使用者會達不到預設的執行緒(使用者)數量。
* **迴圈次數(Loop Count)**: 每個執行緒(使用者)被測試的次數。本案設定6次，表示每個執行緒(使用者)被測試6次，然而此項目會受"期間(Duration)"、"計時器(Timer)"影響，有可能導致有些執行緒(使用者)低於6次，但至少每個執行緒(使用者)都會被測試。相反的，若設定"永久(Infinite)"，則可能導致部分執行緒(使用者)被測試過多，有些執行緒(使用者)卻完成沒測試到的情況。
* **Same user on each iteration**: If we check **"Same User in each Iteration"** then JMeter will use same user details to run the test in each iteration. If we don’t want that then we can leave this field unchecked.
* **Delay thread creation until needed**: 延遲執行緒(使用者)建立，直到需要時才會被建立，勾選後對於執行緒(使用者)數而言,是節省系統資源的，尤其當執行緒(使用者)過大，啟動時間短，勾選就是1個執行緒(使用者)跑完後，下一個執行緒(使用者)接著跑，不勾選就是啟動所有執行緒。有啟動與沒啟動差別，將導致系統資源使用率有所差別，如下圖 (兩者於記憶體使用上有所差異):

![image](https://user-images.githubusercontent.com/14270012/84728278-75ea6300-afc3-11ea-9e21-ae03c193f773.png)

reference: https://www.blazemeter.com/blog/how-to-use-the-delay-thread-creation-on-jmeter

* **定時器-期間(Duration/s)**: 即整體測試時間，本案為30分鐘(1800秒)。
* **定時器-啟動延遲時間(Startup Delay/s)**: 即整體測試遞延啟動的時間。

## HTTP要求(HTTP Request):
![image](https://user-images.githubusercontent.com/14270012/84729477-628cc700-afc6-11ea-9344-4bf6f0a6e30f.png)
* **協定(Protocol)**: 即被測試對象所需要用的通訊協定，如:http、https、FTP..。
* **主機名稱或IP(ServerName or IP)**: 即被測試對象的主機名稱(網址)或IP。
* **端口號碼(Port Number)**: 即被測試對象的Port，如:80、443。
* **方法(Method)**: 即連結被測試對象所使用的http method，如:Get、Post...。
* **路徑(Path)**: 即被測試對象的連結路徑，如:ImageDocWebService/api/XXXXX。
* **Body Data**: 即相關測試資料，可用json格式，但建議另外加入Http Header Manager，設定Content-Type為application/json。
![image](https://user-images.githubusercontent.com/14270012/84738132-ea310080-afdb-11ea-858d-2a2fb029c536.png)

## CSV資料設定(CSV Data Set Config): 
 * 用途: 主要可以將測試之資料放置csv檔，供Jmeter壓測使用。 
 * **檔名(FileName)**: 即csv檔案所存放之路徑。
 * **檔名(Variable Name)**: 建立變數名稱，讓Jmeter使用此變數呼叫csv檔案。本案測試之影像資料，因轉換成base64格式，即可另外存放成csv檔，並讓Jmeter使用變數呼叫，本案所使用的變數IDCardImage，即在上述的**HTTP要求(HTTP Request)** 的 **Body Data** 中使用
 ![image](https://user-images.githubusercontent.com/14270012/84742635-55ca9c00-afe3-11ea-98e1-99ba01539f79.png)

## 計時器(Timer):
 * **固定值計時器(Constant Timer)**: 即每個request固定延遲的時間。
 舉例:
 假設request1的 Start time 是 22:05:01.866, Sample Time of request1 is 172 ms,Constant Timer: 5000 ms
 那最後這個request1結束時間: 22:05:01.866 + 172 + 5000 = 22:05:07.038
 (詳: https://www.guru99.com/timers-jmeter.html, Run your test 章節)
 
 以本案執行緒數量(Number of Threads(users))設定500，搭配constant timer:5000ms，表示同一個user所發送不同的request會固定差異**5000ms+取樣的時間* 。
 
 * **一致隨機計時器(Uniform Random Timer)**: 即這個thread跟下一個thread隨機延遲的時間。以本案的**隨機延遲最大值** 20000(ms)，**常數延遲差**: 60000(ms)，隨機延遲時間60 ~ 80(60+20)秒之間。 
   * [Uniform Random Timer Usage](http://www.software-testing-tutorials-automation.com/2016/12/jmeter-uniform-random-timer-usage.html)
 
 以本案執行緒數量(Number of Threads(users)) 設定500，搭配 Uniform Random Timer: 20000 ~ 80000ms ，表示同一個user所發送不同的request會隨機差異**60 ~ 80秒+取樣的時間** 內。
 
 ![image](https://user-images.githubusercontent.com/14270012/84762744-ca123900-affd-11ea-8e23-31edc165ad85.png)
 
 * **固定時隔(Constant Throughtput Timer)**: 即每分鐘固定多少request，會因上述的**啟動延遲(Ramp-up period)** 、**固定值計時器(Constant Timer)**、 **一致隨機計時器(Uniform Random Timer)** ..等設定的搭配而影響最後測試數據，通常適合用來壓測系統可以承載多大量的requests。
   * [Constant Throughput Timer Example](http://www.software-testing-tutorials-automation.com/2017/02/jmeter-constant-throughput-timer-example.html)
   * [Example of Constant Throughput Timer (sample usage)](http://2min2code.com/articles/jmeter_intro/throughput_timer)
   * [How to use JMeter's Constant Throughput Timer](https://www.blazemeter.com/blog/how-use-jmeters-throughput-constant-timer)
 
 * **Precise Throughtput Timer**: 相較固定時隔，可更精確控制單一時間內測試的request數量，其中除了**Target throught**、**Througput period** 與 **Test Duration** 設定有關外，也會因上述的**啟動延遲(Ramp-up period)** 、**固定值計時器(Constant Timer)**、 **一致隨機計時器(Uniform Random Timer)** ..等設定的搭配而影響最後測試數據，通常適合用來壓測系統可以承載多大量的requests。
 
 
## 重點Jmeter教學:
 * [JMeter官方文件](https://jmeter.apache.org/index.html)
 * [What is JMeter? Introduction & Uses](https://www.guru99.com/introduction-to-jmeter.html)

 

 
 
