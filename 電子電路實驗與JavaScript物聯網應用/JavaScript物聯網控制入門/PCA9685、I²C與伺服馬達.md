###### tags: `js-iot` 返回[電子電路實驗與 JavaScript 物聯網應用](/s/8Q89ww-EQlOg8zy7XSTlCw)

# PCA9685、I²C 與伺服馬達

**這節將介紹可以一次控制多個輸出的 PCA9685，認識其通訊所使用的 I²C 通訊協定，以及 PWM 是如何控制伺服馬達運作。**

## 什麼是 PCA9685 ？

<center><img src="https://i.imgur.com/xVBJiNf.jpg " width="80%"></img></center>

<br>

PCA9685 是採用**I²C 通訊介面**的**16 路 PWM 產生器**，解析度為**12bit**，開關**頻率為 24 Hz 到 1526 Hz**。

雖然具備 16 路的 PWM，但**16 路的頻率無法單獨設置**，只能調整各路的 duty cycle，所以比較適合應用在**固定頻率但需調整 PWM 占空比的 LED 亮度調整**或採用**多軸小型伺服馬達**的機器人。

PCA9685 包含一個帶有**內置時鐘的 I²C 控制 PWM 驅動器**，每次設置皆會**保持最後一次所設置的參數運作**，**無需連續發送訊號控制**。

:::warning
:zap: 圖中左側 VCC 供電屬邏輯電路電源，在 LAB 中使用 3.3V，而上方 V+對應到下方的 V+，為供應伺服馬達的電源，在 LAB 實作中使用 12V。
:::

:::info
:globe_with_meridians: 參考資訊

- [16 路 12bit PWM 控制器 PCA9685](https://frank1025.pixnet.net/blog/post/349725877-%5Barduino-013%5D-16%E8%B7%AF12bit-pwm%E6%8E%A7%E5%88%B6%E5%99%A8pca9685)
- [PCA9685 16 Channel 12 Bit PWM Servo Driver](http://wiki.sunfounder.cc/index.php?title=PCA9685_16_Channel_12_Bit_PWM_Servo_Driver)
  :::

## 什麼是 I²C？

![](https://microcontrollerslab.com/wp-content/uploads/2016/11/I2C-Communication-timing-diagram-pic-interfacing.png)

**I²C(Inter-Integrated Circuit)** 是**內部整合電路**的稱呼，是一種**串列通訊匯流排**，由 Philips 公司在 1980 年代為了讓主機板、手機及嵌入式系統用以連接低速周邊裝置而發展，主要應用在 board-to-board，它的設計並**不能應用到長距離裝置的通訊**。不過，I2C bus 可以被**應用在各種控制架構**上，如系統管理匯流排(System Management Bus, SMBus)、電源管理匯流排(Power Management Bus, PMBus)、智慧平台管理介面(Intelligent Platform Management Interface, IPMI)、顯示數據通道(Display Data Channel, DDC)、先進電信運算架構(Advanced Telecom Computing Architecture, ATCA)。

I²C 的參考設計使用一個**7 位元長度**的位址空間但**保留了 16 個位址**，所以在一組匯流排**最多可和 112 個節點通訊**。常見的 I²C 匯流排**依傳輸速率的不同而有不同的模式**：標準模式（100 Kbit/s）、低速模式（10 Kbit/s），但時脈頻率可被允許下降至零，這代表可以暫停通訊。而新一代的 I²C 匯流排可以和更多的節點（支援 10 位元長度的位址空間）以更快的速率通訊：快速模式（400 Kbit/s）、高速模式（3.4 Mbit/s）。

:::info
:bulb: I²C 的正確讀法為「I 平方 C」（"I-squared-C"），而「I 二 C」（"I-two-C"）則是另一種錯誤但被廣泛使用的讀法。
:::

:::info
:globe_with_meridians: 參考資訊

- [I2C - 成大資工 Wiki](http://wiki.csie.ncku.edu.tw/embedded/I2C)
- [I2C 協定原理簡介](https://makerpro.cc/2019/12/intro-to-inter-integrated-circuit/)
  :::

## PWM 如何控制伺服馬達？

<center><img src="https://i.imgur.com/JdbAYJZ.jpg" width="70%"></img></center>

<br>

在[PWM 與電晶體](/s/iBzaKT_rQb2OzkyF3FJLwA)這個章節中提到 PWM 是透過**類比訊號轉換成數位訊號**，在**一定的週期內調整 PWM 的脈波寬度**，而改變輸出的功率。

LAB 中所使用到的伺服馬達為**MG996R**，使用**50HZ**的控制頻率，脈寬為**0.5ms ~2.5ms**，**12 位元解析度(4096)**，相關精度計算如下：

- PWM 週期： 1/50 sec = 0.02s = 20ms = 20000us
- 時間解析度：20000 us / 4096 = 4.88us
- 最大與最小脈寬時間差：2.5ms - 0.5ms = 2ms = 2000us
- 最大脈寬時間可分的份數：2000us / 4.88us = 410
- 0-180 度的舵機，角度解析度：180 度 / 410 = 0.439 度

以下圖來看在一個週期內波的寬度對伺服馬達的影響，脈寬為 1(0.5)ms 時馬達轉角為 0 度，脈寬為 2(2.5)ms 時馬達轉角為 180 度，故脈寬為 1.5ms 時馬達轉角為 90 度。

![](https://i.imgur.com/qflBthr.png)
(圖片來源：[howtomechatronics](https://howtomechatronics.com/how-it-works/how-servo-motors-work-how-to-control-servos-using-arduino/))

:::info
:globe_with_meridians: 參考資源

- [PCA9685 控制 Servo 伺服馬達](https://atceiling.blogspot.com/2019/10/arduino74pca9685servo.html)
- [MG996R DataSheet](https://www.electronicoscaldas.com/datasheet/MG996R_Tower-Pro.pdf)
  :::

<a class="btn btn-warning" style="width:100%;color:#333333;"  href="/s/9ntHyjh0RBWR2NewpjNQLw" role="button"> LAB04 - 控制二軸機械手臂 **&#8680;** </a>

<a class="btn btn-primary" style="width:100%;"  href="/s/W436RjpIQQCR6mynrvVRGA" role="button"> **&#8678;** LAB03 - 電晶體與呼吸燈
</a>
