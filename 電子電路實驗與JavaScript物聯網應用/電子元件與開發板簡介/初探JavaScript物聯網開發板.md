###### tags: `js-iot` 返回[電子電路實驗與 JavaScript 物聯網應用](/s/8Q89ww-EQlOg8zy7XSTlCw)

# 初探 JavaScript 物聯網開發板

**JavaScript 物聯網開發板為尚晉機電有限公司所設計的實驗板，主要為提供程式學習、人機互動及可客製化的物聯網解決方案。**

**使用的晶片為 ESP-12S，有關晶片的資訊可參考以下連結。**

:::info
:globe_with_meridians: 參考資源

[ESP8266 開發板 - 維基百科](https://zh.wikipedia.org/wiki/ESP8266%E5%BC%80%E5%8F%91%E6%9D%BF)
[ESP-12S 規格書](https://www.laskarduino.cz/user/related_files/esp-12s.pdf)
:::

## 開發板簡介

<center> <img src="https://i.imgur.com/c3mDorI.jpg" width="30%" /> </center>

<br>

- 提供**獨立 WiFi(AP 模式)及區網(AP+STA 模式)連線**，能在**無網路環境**使用，也能連上外部網路，透過**區網進行控制**。
- 提供 I2C、UART、GPIO 介面供輸出及輸入。
- 保留可擴充空間，能**整合多種控制器與感測器**。
- 網頁伺服器功能，能託管小型網頁檔案，**依據不同需求或個人喜好可調整網頁介面**。
- 可將**Arduino 等微控器使用 WiFi 進行控制**。
- 上電即可使用，**不用安裝任何編譯器或軟體**。

:::warning

### :signal_strength: 什麼是 AP ? STA?

#### Access Point(AP)

提供無線接入服務，允許其它無線設備接入，提供數據訪問，一般的無線路由/網橋工作在該模式下。 AP 和 AP 之間允許相互連接。它提供以無線方式組建無線局域網 WLAN，相當際 WLAN 的中心設備。

#### AP+Station(STA)

當一個 WIFI 芯片提供這個功能時，它們可以連到另外的一個網絡當中，如家用路由器就是這種，AP MODE 提供給手機設備等連接，提供上網功能，實際能提供上網功能的就是 STATION MODE 做為 INTERNET 的一個工作站，所以 STATION MODE 通常用於提供網絡的數據上行服務。在芯片支持的前提下，LINUX 或 EMBEDDED LINUX SYSTEM 都提供了相應的驅動支持，可以使兩種模式同時存在，同時工作。

來源連結 - [WiFi 有兩種 mode: STA 與 AP (又叫作 SoftAP,HostAP)](https://eeepage.info/wifimode-sta-ap/)
:::

## 開發板電路圖

![circuit](https://i.imgur.com/DkPiLc9.png)

## 開發板電子元件及腳位一覽

|                                       名稱                                       |                   實體圖                   |                               用途                               |
| :------------------------------------------------------------------------------: | :----------------------------------------: | :--------------------------------------------------------------: |
|                                     ESP-12S                                      | ![](https://i.imgur.com/mVVmBIM.png =30%x) |                 網頁伺服器，WiFi，IO 輸入輸出。                  |
| [貼片電阻](https://www.easyatm.com.tw/wiki/%E8%B2%BC%E7%89%87%E9%9B%BB%E9%98%BB) | ![](https://i.imgur.com/hCaqCbn.png =20%x) |                      焊接在電路板上的電阻。                      |
|                                     ADC 輸入                                     | ![](https://i.imgur.com/6YHhdzS.png =30%x) | 位於電路板上 J2，將類比訊號轉換成數位訊號，最大輸入電壓為 3.3V。 |
|                                     RST 按鈕                                     | ![](https://i.imgur.com/Dttd3aF.png =30%x) |      重置開發板狀態，當 WiFi 名稱更新或異常狀況發生時使用。      |
|                                 AMS117 穩壓模組                                  | ![](https://i.imgur.com/mKEF1Q1.png =40%x) |           將 12~24V 的電壓，降為 3.3V 供應給 ESP-12S。           |
|              [電容](http://koshin.com.hk/news/shownews.php?id=128)               | ![](https://i.imgur.com/jUklpli.png =40%x) |               防止上電時瞬間電流過大損壞電子元件。               |
|   [UART](https://makerpro.cc/2016/07/learning-interfaces-about-uart-i2c-spi/)    | ![](https://i.imgur.com/iFRJWIe.png =40%x) |                         UART 通訊腳位。                          |
|    [I2C](https://makerpro.cc/2016/07/learning-interfaces-about-uart-i2c-spi/)    | ![](https://i.imgur.com/T1YS2AX.png =30%x) |                          I2C 通訊腳位。                          |
|                                      GPIO 1                                      | ![](https://i.imgur.com/jYKx5WI.png =30%x) |      用於選擇類比數位多工器的腳位(4 個訊號選擇 16 個訊號)。      |
|                                      GPIO 2                                      | ![](https://i.imgur.com/5fO2BoF.png =30%x) |                          作為預留腳位。                          |
|                                     擴充腳位                                     | ![](https://i.imgur.com/evRgGQE.png =40%x) |                        預留擴充其他模組。                        |
|                                     電源輸入                                     | ![](https://i.imgur.com/ZmgamQN.png =40%x) |               提供主板電源，接受 12~24V 電源輸入。               |

<a class="btn btn-warning" style="width:100%;color:#333333;"  href="/s/r8SqOJDqQlGaYIUxYinQHw" role="button"> 開發板網頁功能簡介 **&#8680;** </a>

<a class="btn btn-primary" style="width:100%;"  href="/s/UFTbsN1xSA2hovuJ-4xNBw" role="button"> **&#8678;** 初探電子元件模組
</a>
