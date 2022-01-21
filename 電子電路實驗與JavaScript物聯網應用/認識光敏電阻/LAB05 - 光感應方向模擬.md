---
title: LAB05 - 光感應方向模擬
slideOptions:
  transition: slide
  transitionSpeed: "fast"
  theme: league
---

<!-- .slide: data-background="https://i.imgur.com/cgViIs1.jpg" data-background-color="#111111" data-background-opacity="0.2" -->

###### tags: `js-iot` `lab` 返回[電子電路實驗與 JavaScript 物聯網應用](/s/8Q89ww-EQlOg8zy7XSTlCw)

## LAB 05 <br> <span style="color:#F9BF45;">光感應方向模擬</span>

###### [點我開啟簡報模式](/@BEExANT-ta/BkEsjBASF#)

###### <kbd>ESC</kbd> 鍵進入總覽模式

###### <kbd>&#8592;</kbd> <kbd>&#8593;</kbd> <kbd>&#8595;</kbd> <kbd>&#8594;</kbd> 切換頁面

---

## 目標

**讀取 4 個光敏電阻值，判斷出 ADC 值高的方向(較亮)，開啟對應方向的 LED，當 4 個值都很接近時，將 LED 全關表示狀態平衡。**

---

## 使用材料

- **JavaScript 開發板**
- **220、1KΩ 電阻**
- **光敏電阻**
- **LED**
- **太陽能壓克力模組**
- **杜邦線**

---

## 線路圖

:::warning
:zap: 圖上麵包板正負極與實際不同，需注意正負極
:::

---

<center> <img src="https://i.imgur.com/jTVKgni.png" width="75%"></img> </center>

---

## 設計原理

- 以==兩個電阻 ADC 值相加為一組==，分成==上下左右四個方向==，左與右進行比較，值較==大則開啟對應方向的 LED==，上下也進行比較，同時==最多只有兩個 LED 開啟==(左上、左下、右上、右下)，==全關==表示四個電阻 ADC 值在==容許的誤差範圍==內。

---

## 範例程式

新增程式檔並命名 ==LAB05 - 光感應方向模擬==，將以下程式碼複製貼上程式編輯區執行。

```javascript=
let up = DATA.adc[0] + DATA.adc[1];
let down = DATA.adc[2] + DATA.adc[3];
let left = DATA.adc[0] + DATA.adc[2];
let right = DATA.adc[1] + DATA.adc[3];
let range = 20;

if((up > down) && (up - down) > range) {
  console.log("光線在上方較強！");
  pwm[0].set(100);
  delay(300);
  pwm[1].set(0);
  delay(300);
}
if((down > up) && (down - up) > range) {
  console.log("光線在下方較強！");
  pwm[1].set(100);
  delay(300);
  pwm[0].set(0);
  delay(300);
}
if(Math.abs(up - down) <= range) {
  console.log("光線在上下已達平衡！");
  pwm[0].set(0);
  delay(300);
  pwm[1].set(0);
  delay(300);
}

if((left > right) && (left - right) > range) {
  console.log("光線在左方較強！");
  pwm[2].set(100);
  delay(300);
  pwm[3].set(0);
  delay(300);
}
if((right > left) && (right - left) > range) {
  console.log("光線在右方較強！");
  pwm[3].set(100);
  delay(300);
  pwm[2].set(0);
  delay(300);
}
if(Math.abs(left - right) <= range) {
  console.log("光線在左右已達平衡！");
  pwm[2].set(0);
  delay(300);
  pwm[3].set(0);
  delay(300);
}
```

---

## 程式解說

逐行解釋程式意義。

---

```javascript=
let up = DATA.adc[0] + DATA.adc[1];
let down = DATA.adc[2] + DATA.adc[3];
let left = DATA.adc[0] + DATA.adc[2];
let right = DATA.adc[1] + DATA.adc[3];
```

- 以區域變數定義四個方向，每一變數中的值為兩個光敏電阻的 ADC 值相加。

---

```javascript=5
let range = 20;
```

- 以區域變數定義誤差範圍值。

---

```javascript=7
if((up > down) && (up - down) > range) {
  pwm[0].set(100);
  delay(300);
  pwm[1].set(0);
  delay(300);
}
```

- 當 **上大於下 且 上減去下 大於** 誤差值時。

1. 開啟第一個 LED 燈(綠)。
2. 等待 300 毫秒。
3. 關閉第二個 LED 燈(綠)。
4. 等待 300 毫秒。

==表示光線在上方較強==

---

```javascript=13
if((down > up) && (down - up) > range) {
  pwm[1].set(100);
  delay(300);
  pwm[0].set(0);
  delay(300);
}
```

- 當 **下大於上 且 下減去上 大於** 誤差值時。

1. 開啟第二個 LED 燈(綠)。
2. 等待 300 毫秒。
3. 關閉第一個 LED 燈(綠)。
4. 等待 300 毫秒。

==表示光線在下方較強==

---

```javascript=19
if(Math.abs(up - down) <= range) {
  pwm[0].set(0);
  delay(300);
  pwm[1].set(0);
  delay(300);
}
```

- 使用**Math.abs**這個函式將**上減去下的差取絕對值**。
- 判斷值是否 **小於等於** 誤差值。

1. 關閉第一個 LED 燈(綠)。
2. 等待 300 毫秒。
3. 關閉第二個 LED 燈(綠)。
4. 等待 300 毫秒。

==表示光線在上下已取得平衡==

---

```javascript=26
if((left > right) && (left - right) > range) {
  pwm[2].set(100);
  delay(300);
  pwm[3].set(0);
  delay(300);
}
```

- 當 **左大於右 且 左減去右 大於** 誤差值時。

1. 開啟第三個 LED 燈(紅)。
2. 等待 300 毫秒。
3. 關閉第四個 LED 燈(紅)。
4. 等待 300 毫秒。

==表示光線在左方較強==

---

```javascript=32
if((right > left) && (right - left) > range) {
  pwm[3].set(100);
  delay(300);
  pwm[2].set(0);
  delay(300);
}
```

- 當 **右大於左 且 右減去左 大於** 誤差值時。

1. 開啟第四個 LED 燈(紅)。
2. 等待 300 毫秒。
3. 關閉第三個 LED 燈(紅)。
4. 等待 300 毫秒。

==表示光線在右方較強==

---

```javascript=38
if(Math.abs(left - right) <= range) {
  pwm[2].set(0);
  delay(300);
  pwm[3].set(0);
  delay(300);
}
```

- 使用**Math.abs**這個函式將**左減去右的差取絕對值**。
- 判斷值是否 **小於等於** 誤差值。

1. 關閉第三個 LED 燈(綠)。
2. 等待 300 毫秒。
3. 關閉第四個 LED 燈(綠)。
4. 等待 300 毫秒。

==表示光線在左右已取得平衡==

---

## 參數修改

為方便實作，以下會將範例程式中可修改的參數標示出來，進行實作時只需修改對應參數，並觀察結果即可。

:::warning
:zap: 詳細內建 JS 參數參考 - [內建 Js 參數及功能總覽](/s/51F8nDF3Ss6M2DmGoRJQbA)
:::

---

:::success
**let range = ==x==**
:::

- x 為誤差範圍值，意思是光敏電阻間的差值在這個範圍內表示平衡，建議為 20~50，依據環境不同進行調整。

---

:::success
**DATA.adc[==x==]**
:::

- x 可代入 0 ~ 15，表示取得第 1 ~ 16 個腳位的 ADC 資料，依據使用的腳位填入對應的數字。

---

:::success
**pwm[==x==],set(==y==);**
:::

- x 可代入 0 ~ 7，表示第 1 ~ 8 個 PWM 輸出腳位，y 可代入 0 ~ 100，表示 PWM 輸出的大小。

---

## 範例影片(待更新)

{%youtube MZ8Ze4xr2DA %}

<a class="btn btn-warning" style="width:100%;color:#333333;"  href="/s/l41cgJCKSDi3mo_kf1AsXA" role="button"> 太陽能光追蹤系統原理 **&#8680;** </a>

<a class="btn btn-primary" style="width:100%;"  href="/s/z89HDGusTD-PnaB04yM4qQ" role="button"> **&#8678;** 類比數位訊號多工器原理
</a>
