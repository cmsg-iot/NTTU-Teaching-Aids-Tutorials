###### tags: `js-iot` 返回[電子電路實驗與 JavaScript 物聯網應用](/s/8Q89ww-EQlOg8zy7XSTlCw)

# PWM 與電晶體

**在[LAB02 - 按鈕控制 LED](/s/ViJQSKkDTgKc0X_is4fOAQ)中我們使用 PWM 輸出控制 LED 燈的開關，這節將認識什麼是 PWM，以及後續在[LAB03 - 電晶體與呼吸燈](/s/W436RjpIQQCR6mynrvVRGA)中使用到的電晶體。**

## 什麼是 PWM？

<center><img src="https://www.newton.com.tw/img/3/484/cGcq5yMjFzM0IDOwYTMwQmN2IWNzUmZzIzMldjYjFDOhRWM1QzYkRWZ2MWYv0WZ0l2LjlGcvU2apFmYv02bj5SdklWYi5yYyN3Ztl2LvoDc0RHa.jpg" width="60%"></img></center>

<br>

**脈波寬度調變（英語：Pulse-width modulation，縮寫：PWM）**，簡稱脈寬調變，是將**類比訊號轉換為脈波**的一種技術，一般轉換後脈波的週期固定，但**脈波的工作週期會依類比訊號的大小而改變**。

在類比電路中，類比訊號的值可以**連續進行變化**，在時間和值的幅度上都幾乎沒有限制，基本上可以取任何實數值，**輸入與輸出也呈線性變化**。所以在類比電路中，**電壓和電流可直接用來進行控制對象**，例如家用電器設備中的**音量開關控制**、採用鹵素燈泡燈具的**亮度控制**等等。

與類比電路不同，**數位電路是在預先確定的範圍內取值**，在任何時刻，其**輸出只可能為 ON 和 OFF**兩種狀態，所以電壓或電流會通/斷方式的重複脈波序列加載到類比負載。

PWM 技術是一種對**類比訊號電位的數位編碼方法**，通過使用高解析度計數器（調變頻率）**調變方波的占空比**，從而實現對一個類比訊號的電位進行編碼。其最大的優點是從**處理器到被控對象之間的所有訊號都是數位形式**的，無需再進行數位類比轉換過程；而且對**雜訊的抗干擾能力也大大增強**（雜訊只有在強到足以將邏輯值改變時，才可能對數位訊號產生實質的影響），這也是 PWM 在通訊等訊號傳輸行業得到大量應用的主要原因。

最簡單可以產生一個脈衝寬度調變訊號的方式是**交集性方法(intersective method)**，這個方法只需要**使用鋸齒波或三角波**(可以簡單地使用震盪器來產生)，以及一個**比較器**。當參考的訊號值(下圖的紅色波)比鋸齒波(下圖的藍色波)來的大，則脈衝調變後的結果會在高狀態，反之，則在低狀態。

![](https://i.imgur.com/WSsyd7K.png)

:::info
:bulb: 數位輸出只會有 0 跟 1，如果想輸出 0.5 要怎麼做呢？ 在一個週期內輸出 01010101...，平均後就會得到 0.5 了(佔空比 50%)。在 LAB 中所使用到的程式 **pwm[0].set(0)** 與 **pwm[0].set(100)** 就是代表數位輸出 0 與 1 的意思喔！
:::

![](https://i.imgur.com/ArmyxOX.png)

:::warning
:zap: 可以使用示波器觀察 PWM 的波型。
:::

:::info
:globe_with_meridians: 參考資源

- [脈波寬度調變 - 維基百科](https://zh.wikipedia.org/wiki/%E8%84%88%E8%A1%9D%E5%AF%AC%E5%BA%A6%E8%AA%BF%E8%AE%8A)
- [Pulse-Width Modulation (PWM) - 成大資工 Wiki](http://wiki.csie.ncku.edu.tw/embedded/PWM)
  :::

## ULN2803 達靈頓電晶體陣列

### 什麼是 ULN2803？

<center><img src="https://i.imgur.com/KcTCogD.jpg" width="50%"></img></center>

ULN2803 是芯片、集成電路，與許多其他產品一樣，採用傳統的 DIP 包裝，在其側面上有兩疊針腳。

![](https://www.hwlibre.com/wp-content/uploads/2020/03/uln2803-ic-transistor-darlingon.jpg.webp)

:::info
:globe_with_meridians: 參考資源

- [ULN2803：關於達林頓晶體管對](https://www.hwlibre.com/zh-TW/uln2803/)
- [ULN2803A DataSheet](https://www.ti.com/lit/ds/symlink/uln2803a.pdf)
  :::

### 什麼是達靈頓電晶體？

**達靈頓電晶體(英語：Darlington transistor)，或稱達靈頓對(Darlington pair)** 是電子學中由兩個（甚至多個）雙極性電晶體（或者其他類似的積體電路或分立元件）組成的複合結構，通過這樣的結構，**經第一個雙極性電晶體放大的電流可以進一步被放大**。

這樣的結構可以**提供一個比其中任意一個雙極性電晶體高得多的電流增益**。

在使用集成電流晶片的情況里，達靈頓電晶體可以使得晶片比使用兩個分立電晶體元件占用更少的空間，因為**兩個電晶體可以共用一個集極**。達靈頓電晶體通常被封裝在單一的晶片里，從外面看就像一個雙極性電晶體。

![](https://i.imgur.com/dJGSoNM.png)

:::info
:globe_with_meridians: 參考資源

- [達靈頓電晶體 - 維基百科](https://zh.wikipedia.org/wiki/%E8%BE%BE%E7%81%B5%E9%A1%BF%E6%99%B6%E4%BD%93%E7%AE%A1)
- [三極體（電晶體）Transistor](https://www.phy.ntnu.edu.tw/demolab/electronics/transistor.html)
  :::

<a class="btn btn-warning" style="width:100%;color:#333333;"  href="/s/W436RjpIQQCR6mynrvVRGA" role="button"> LAB03 - 電晶體與呼吸燈 **&#8680;** </a>

<a class="btn btn-primary" style="width:100%;"  href="/s/ViJQSKkDTgKc0X_is4fOAQ" role="button"> **&#8678;** LAB02 - 按鈕控制 LED
</a>
