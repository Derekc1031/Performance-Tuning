# 使用Jmeter進行負載壓力測試 
#### 案例要求: 證件辨識API壓測，500個使用者執行交易，每位使用者依序發出request，同一使用者間隔60~100秒，測試時間30分鐘，系統平均回應時間不得高於5秒。

#### Jmeter重點參數設定：

* **執行緒數量(Number of Threads(users))**: 就是使用者數量，本案例為500個使用者。
* **啟動延遲(Ramp-up period)**: 多久時間內達到執行緒(使用者)數量。本案設定1500秒衝到500個使用者，若設定太短會使系統負載過重，使得結果失真，但設定太長則會使系統負載過輕，壓不出系統之瓶頸。本案例整體測試時間30分鐘(1800秒)，不建議將此設定為1800秒(整體測試時間)，因為Jmeter特性，會在接近1800秒時讓使用者數量快速下降，導致最後壓測使用者會達不到預設的執行緒(使用者)數量。
* **迴圈次數(Loop Count)**: 每個執行緒(使用者)被測試的次數。本案設定6次，表示每個執行緒(使用者)被測試6次，然而此項目會受"期間(Duration)"、"計時器(Timer)"影響，有可能導致有些執行緒(使用者)低於6次，但至少每個執行緒(使用者)都會被測試。相反的，若設定"永久(Infinite)"，則可能導致部分執行緒(使用者)被測試過多，有些執行緒(使用者)卻完成沒測試到的情況。


![image](https://user-images.githubusercontent.com/14270012/84615441-9cdd6200-aefb-11ea-9a5b-cd0f42daea29.png)


