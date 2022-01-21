---
title: LAB01 - ADC電壓量測實驗
slideOptions:
  transition: slide
  transitionSpeed: "fast"
  theme: league
---

<!-- .slide: data-background="https://i.imgur.com/cgViIs1.jpg" data-background-color="#111111" data-background-opacity="0.2" -->

###### tags:`js-iot` `lab` 返回[電子電路實驗與 JavaScript 物聯網應用](/s/8Q89ww-EQlOg8zy7XSTlCw)

## LAB 01 <br> <span style="color:#F9BF45;">ADC 電壓量測實驗</span>

###### [點我開啟簡報模式](/@BEExANT-ta/S1pZrairK#)

###### <kbd>ESC</kbd> 鍵進入總覽模式

###### <kbd>&#8592;</kbd> <kbd>&#8593;</kbd> <kbd>&#8595;</kbd> <kbd>&#8594;</kbd> 切換頁面

---

## 目標

- 實驗一 - 分壓計算與量測
- 實驗二 - LED 電壓量測

---

### :dart: 實驗一 - 分壓計算與量測

**將兩個相同大小的電阻串聯分壓，計算出應該得到的電壓值，再使用 ADC 輸入量測電壓，並替換不同大小電阻，觀察數據並理解爲何量測與計算值不同。**

---

### :dart: 實驗二 - LED 電壓量測

**使用 ADC 輸入腳位量測電壓，在網頁取得 ADC 輸入資料，換算出電壓，調整可變電阻並觀察 LED 亮度與電壓變化，記錄電壓與 LED 亮度關係。**

---

## 使用材料

- **JavaScript 開發板**
- **220、1K、10K、100KΩ 電阻**
- **100KΩ 可變電阻** (參考[電阻色碼表](/s/ZjKg95nBRVCVy3V6aPNYgQ))
- **紅、藍、綠 LED**
- **杜邦線**

---

## 線路圖

:::warning
:zap: 圖上麵包板正負極與實際不同，需注意正負極
:::

---

#### 實驗一，分壓量測(100K 電阻 \* 2)

<center> <img src="https://i.imgur.com/nVHbaVz.png" width="68%"></img> </center>

---

#### 實驗二，LED 電壓量測(1K 電阻 \* 1)

<center> <img src="https://i.imgur.com/BWHhYTo.png" width="70%"></img> </center>

---

## 設計原理

- ADC 轉換電壓公式(Vc: 轉換電壓, ADC: 量測 ADC 值, Vr: 輸入電壓, n bits ADC, 一般爲 10 bits)。

\begin{gather*}V_c = ADC \times V_r \times (2^n - 1)\end{gather*}

---

- 使用 ADC 量測電壓時，==主控板 signal 偵測電阻值(位於 J1)需大於量測電阻值==(偵測>量測，越大越準確)。如下圖 ==100K(R1) > 22K + 10K(R2)==，並聯後的電阻值僅剩==24.24K==(依公式計算)，使用 ADC 量測出約 0.7V，與實際 1.66V 相差過大。

<center> <img src="https://i.imgur.com/aXNIjsL.png"></img> </center>

---

- 並聯和串聯電阻計算公式

\begin{gather*}R_T = {\cfrac{1}{(\cfrac{1}{R_1}+\cfrac{1}{R_2}+\cfrac{1}{R_3}+etc...)} }\end{gather*}

:::info
:globe_with_meridians: 參考資源

- [ADC 轉換公式](http://wiki.csie.ncku.edu.tw/embedded/ADC#adc-%E8%BD%89%E6%8F%9B%E5%85%AC%E5%BC%8F)
- [並聯和串聯電阻計算器
  ](https://www.digikey.tw/zh/resources/conversion-calculators/conversion-calculator-parallel-and-series-resistor)
  :::

---

- 利用可變電阻改變電壓，透過 ADC 量測取得電壓(LED 壓降 0.7V)，記錄==不同電壓下對不同 LED 的亮度變化==。
  :::warning
  :zap: LED 最佳工作電流為 35mA 以下，超過容易損耗，而實際三用電表量測會有些許誤差，需經過校正才更精確(ADC 量測最大爲 3.3V)。
  :::

---

- 電壓與 LED 間的關係圖(來源:[Testing unknown LEDs](http://lednique.com/test-equipment/testing-unknown-leds/))

<center><img src="https://i.imgur.com/YW89oKj.png"></img> </center>

---

## 範例程式

新增程式檔並命名 ==LAB01 - ADC 電壓量測實驗==，將以下程式碼複製貼上程式編輯區執行。

```javascript=
let V = 3.3;
let offset = 0;
let adc = DATA.adc[0];
let v = 0;

v = (adc - offset) / 1024 * V;

v = Math.round(v * 100) /100;

console.log("電源輸入: " + V);
console.log("量測電壓: " + v);
delay(100);
```

---

## 程式解說

逐行講解程式意義。

---

```javascript=
let V = 3.3;
```

- 定義區域變數 V 為 3.3，表示輸入電壓值。

---

```javascript=
let offset = 0;
```

- 定義區域變數 offset 為 0，為 ADC 校正歸零的偏差值。

---

```javascript=
let adc = DATA.adc[0];
```

- 定義區域變數 adc 為 DATA.adc[0]，意思是 DATA 物件中 adc 陣列的第一個值，代表==第一個腳位的 ADC 資料==。

---

```javascript=
let v = 0;
```

- 定義區域變數 v 為 0，作為儲存輸出電壓值的變數 。

---

```javascript=
v = (adc - offset) / 1024 * V;
```

- 將 ADC 值校正後換算成電壓，把值設給 v。

---

```javascript=
v = Math.round(v * 100) /100;
```

- 將 v 值取到小數後二位。

---

```javascript=
console.log("電源輸入: " + V);
console.log("量測電壓: " + v);
```

- 在訊息輸出區顯示輸入電源(V)與量測電壓(v)。

---

```javascript=
delay(100);
```

- 等待 100 毫秒後繼續執行。

---

## 參數修改

為方便實作，以下會將範例程式中可修改的參數標示出來，進行實作時只需修改對應參數，並觀察結果即可。

:::warning
:zap: 詳細內建 JS 參數參考 - [內建 Js 參數及功能總覽](/s/51F8nDF3Ss6M2DmGoRJQbA)
:::

---

:::success
**offset = ==0==;**
:::

- 依據 ADC 輸入的初始值設定，將其校正歸零。

---

:::success
**adc = DATA.adc[==x==];**
:::

- x 可代入 0 ~ 15，表示取得第 1 ~ 16 個腳位的 ADC 資料，依據使用的腳位填入對應的數字。

---

## 範例影片

{%youtube EAYI-9glMKE %}

---

{%youtube Wgp_tpqvVic %}

<a class="btn btn-warning" style="width:100%;color:#333333;"  href="/s/4CgYHd9BREiq0TwqEWpRKw" role="button"> JavaScript 基礎語法與變數運用 **&#8680;** </a>

<a class="btn btn-primary" style="width:100%;"  href="/s/r8SqOJDqQlGaYIUxYinQHw" role="button"> **&#8678;** 開發板網頁功能總覽
</a>
