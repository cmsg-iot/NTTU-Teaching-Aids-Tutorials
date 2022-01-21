---
title: LAB06 - 太陽能光追蹤系統
slideOptions:
  transition: slide
  transitionSpeed: "fast"
  theme: league
---

<!-- .slide: data-background="https://i.imgur.com/cgViIs1.jpg" data-background-color="#111111" data-background-opacity="0.2" -->

###### tags: `js-iot` `lab` 返回[電子電路實驗與 JavaScript 物聯網應用](/s/8Q89ww-EQlOg8zy7XSTlCw)

## LAB 06 <br> <span style="color:#F9BF45;">太陽能光追蹤系統</span>

###### [點我開啟簡報模式](/@BEExANT-ta/H1vjoHCBY#)

###### <kbd>ESC</kbd> 鍵進入總覽模式

###### <kbd>&#8592;</kbd> <kbd>&#8593;</kbd> <kbd>&#8595;</kbd> <kbd>&#8594;</kbd> 切換頁面

---

## 目標

**利用光敏電阻的數據判斷，使模組上的平臺往手電筒/太陽光最亮的方向移動，並量測太陽能板的電壓，觀察追蹤前後的差異。**

---

## 使用材料

- **JavaScript 開發板**
- **220Ω 電阻**
- **光敏電阻**
- **伺服馬達**
- **太陽能壓克力模組**
- **杜邦線**

---

## 線路圖

:::warning
:zap: 圖上麵包板正負極與實際不同，需注意正負極
:::

---

<center> <img src="https://i.imgur.com/hB13kVH.png" width="75%"></img> </center>

---

## 設計原理

- 以每==兩個光敏電阻表示為一方向==，共取==四個方向==值，==左右==方向影響==X 軸==旋轉，==上下==方向影響==Y 軸==旋轉，使四個方向值維持在一定的範圍內，讓太陽能板輸出的電壓較穩定。

---

- 在模組的==兩側各有一個按鈕==，為模組的==極限開關==，當==按鈕被按下==且==馬達轉向為對應開關的方向==時，==停止馬達==運轉，==例： 正轉時碰到按鈕，此時無法繼續正轉，但可以反轉==。

---

## 注意事項

- 由於壓克力架較為脆弱，實作時需==確保極限開關觸碰時能正確停止馬達運轉==，若在==碰到極限開關還繼續往下運轉則會損壞壓克力架==。
- 實作開始時先將兩個馬達設為 90 度，且上面的面板需置中。

---

## 範例程式

新增程式檔並命名 ==LAB06 - 太陽能光追蹤系統==，將以下程式碼複製貼上程式編輯區執行。

```javascript=
let up = DATA.adc[0] + DATA.adc[1];
let down = DATA.adc[2] + DATA.adc[3];
let left = DATA.adc[0] + DATA.adc[2];
let right = DATA.adc[1] + DATA.adc[3];
let btn_CCW = DATA.adc[4];
let btn_CW = DATA.adc[5];
let range = 20;
let v = Math.round((DATA.adc[15] / 1024 * 3.3)*100)/100;
window.roll = 90;
window.pitch = 90;

if (btn_CW == 1024 && pitch < 90) {
  servo[7].set(90);
  pitch = 90;
  console.log("觸碰正轉極限開關!");
}
if (btn_CCW == 1024 && pitch > 90) {
  servo[7].set(90);
  pitch = 90;
  console.log("觸碰反轉極限開關!");
}
delay(300);

if (left > right && left - right > range) {
  roll = roll + 1;
}
if (right > left && right - left > range) {
  roll = roll - 1;
}
if (Math.abs(left - right) <= range) {
  console.log("左右光感測平衡");
}

servo[6].set(roll);
console.log("roll轉角: " + roll);
delay(300);

if (roll >= 130) {
  roll = 130;
}
if (roll <= 50) {
  roll = 50;
}
if (up > down && btn_CCW != 1024) {
  pitch = 120;
  console.log("反轉下降中...");
}
if (down > up && btn_CW != 1024) {
  pitch = 70;
  console.log("正轉上升中...");
}
if (Math.abs(up - down) < range) {
  pitch = 90;
  console.log("上下光感測平衡");
}
servo[7].set(pitch);
console.log("pitch轉角: " + pitch);
console.log("太陽能板輸出電壓: " + v);
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

- 以區域變數定義==四個方向==，每一變數中的值為==兩個光敏電阻的 ADC 值相加==。

---

```javascript=
let btn_CCW = DATA.adc[4];
let btn_CW = DATA.adc[5];
let range = 20;
let v = Math.round((DATA.adc[15] / 1024 * 3.3)*100)/100;
```

- 以區域變數定義 ==正轉極限開關(CW,預設第 6 個 ADC)== 與 ==反轉極限開關(CCW,預設第 5 個 ADC)== 及誤差範圍值。
- 以區域變數定義 v 為第 16 個腳位的 ADC 轉換電壓值(太陽能板輸出電壓)。

---

```javascript=
window.roll = 90;
window.pitch = 90;
```

- 以全域變數定義 ==roll==(繞 X 軸旋轉) 與 ==pitch==(繞 Y 軸旋轉)，分別表示==左右轉角==與==前後傾角==。
- 由於只能以==指定角度==控制伺服馬達，故需定義全域變數==記錄變化後的角度==。

**roll 值與 pitch 值在這邊對馬達==控制的方式不同==，==roll==為指定馬達==左右轉至指定的角度==，==pitch==則是設定馬達==持續的正轉或反轉==，進而==控制前後傾角==**。

---

```javascript=
if (btn_CW == 1024 && pitch < 90) {
  servo[7].set(90);
  pitch = 90;
  console.log("觸碰正轉極限開關!");
}
if (btn_CCW == 1024 && pitch > 90) {
  servo[7].set(90);
  pitch = 90;
  console.log("觸碰反轉極限開關!");
}
delay(300);
```

- 當==正轉==極限開關被觸發且馬達為==逆時針==旋轉時，停止下方馬達運轉，pitch 值設為 90，並顯示對應訊息。
- 當==反轉==極限開關被觸發且馬達為==順時針==旋轉時，停止下方馬達運轉，pitch 值設為 90，並顯示對應訊息。

---

```javascript=
if (left > right && left - right > range) {
  roll = roll + 1;
}
if (right > left && right - left > range) {
  roll = roll - 1;
}
```

- 當==左大於右== 且 ==左減去右== 大於 誤差值時，roll 的角度==加一==，將模組向==左邊==轉。
- 當==右大於左== 且 ==右減去左== 大於 誤差值時，roll 的角度==減一==，將模組向==右邊==轉。

---

```javascript=
if (Math.abs(left - right) <= range) {
  console.log("左右光感測平衡");
}
```

- 使用==Math.abs==這個函式將==左減去右==的差取絕對值，當值在誤差範圍內時，顯示對應訊息，不對 roll(左右轉角)做任何改變。

---

```javascript=
servo[6].set(roll);
console.log("roll轉角: " + roll);
delay(300);
```

- 將前面運算完的==roll==(左右轉角)值設定給==上方馬達進行控制，並顯示目前 roll 數值。

---

```javascript=
if (roll >= 130) {
  roll = 130;
}
if (roll <= 50) {
  roll = 50;
}
```

- 將==roll==值控制在==一定範圍內(50~130)==，不讓 roll 值超過範圍，防止模組過度旋轉而損壞。

---

```javascript=
if (up > down && btn_CCW != 1024) {
  pitch = 120;
  console.log("反轉下降中...");
}
if (down > up && btn_CW != 1024) {
  pitch = 70;
  console.log("正轉上升中...");
}
```

- 當==上大於下== 且 ==反轉極限==開關==未觸發==時，將==pitch==(前後傾角)設為==120==，代表馬達將以==反轉運作==，並顯示對應訊息。
- 當==下大於上== 且 ==正轉極限==開關==未觸發==時，將==pitch==(前後傾角)設為==70==，代表馬達將以==正轉運作==，並顯示對應訊息。

---

```javascript=
if (Math.abs(up - down) < range) {
  pitch = 90;
  console.log("上下光感測平衡");
}
```

- 使用==Math.abs==這個函式將==上減去下==的差取絕對值，當值在誤差範圍內時，將==pitch==(前後傾角)設為==90(表示停止)==，並顯示對應訊息。

---

```javascript=
servo[7].set(pitch);
console.log("pitch轉角: " + pitch);
console.log("太陽能板輸出電壓: " + v);
```

- 將前面運算完的==pitch==(前後傾角)值設定給==下方馬達==進行控制，並顯示目前 pitch 數值與轉換後的太陽能板電壓值。

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
**roll = roll + ==x==**
**roll = roll - ==x==**
:::

- x 的值至少為 1，代表每次模組左右旋轉的角度，角度越小移動越精確。

---

:::success
**pitch = ==x==**
:::

- x 的值為 90 時表示停止，大於 90 表示反轉，小於 90 表示正轉，與 90 相差越大轉速則越快。

---

## 範例影片

{%youtube F425dfKsWgk %}

---

{%youtube GFqc7d3-tLQ %}

<a class="btn btn-warning" style="width:100%;color:#333333;"  href="/s/8Q89ww-EQlOg8zy7XSTlCw" role="button"> 電子電路實驗與 JavaScript 物聯網應用 **&#8680;** </a>

<a class="btn btn-primary" style="width:100%;"  href="/s/l41cgJCKSDi3mo_kf1AsXA" role="button"> **&#8678;** 太陽能光追蹤系統原理
</a>
