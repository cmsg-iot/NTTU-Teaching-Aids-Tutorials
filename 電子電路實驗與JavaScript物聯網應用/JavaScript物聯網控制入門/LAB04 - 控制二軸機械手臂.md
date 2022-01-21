---
title: LAB04 - 控制二軸機械手臂
slideOptions:
  transition: slide
  transitionSpeed: "fast"
  theme: league
---

<!-- .slide: data-background="https://i.imgur.com/o8ekCWp.jpg" data-background-color="#111111" data-background-opacity="0.2" -->

###### tags: `js-iot` `lab` 返回[電子電路實驗與 JavaScript 物聯網應用](/s/8Q89ww-EQlOg8zy7XSTlCw)

## LAB 04 <br> <span style="color:#F9BF45;">控制二軸機械手臂</span>

###### [點我開啟簡報模式](/@BEExANT-ta/BkQssrRrF#)

###### <kbd>ESC</kbd> 鍵進入總覽模式

###### <kbd>&#8592;</kbd> <kbd>&#8593;</kbd> <kbd>&#8595;</kbd> <kbd>&#8594;</kbd> 切換頁面

---

## 目標

**透過按鈕搭配網頁程式切換手臂控制模式(停止、待機、搬貨)，夾取物體並擺放至另一側。**

---

## 使用材料

- **JavaScript 開發板**
- **二軸機械手臂**
- **按鈕**
- **杜邦線**

---

## 線路圖

:::warning
:zap: 圖上麵包板正負極與實際不同，需注意正負極
:::

---

<center> <img src="https://i.imgur.com/3tAjbKZ.png" width="75%"></img> </center>

---

## 設計原理

- 在網頁上先手動設定伺服馬達的轉角，紀錄並調整手臂在待機及搬貨時的角度。
- ==判斷按鈕的 ADC 輸入==，分別定義==停止、待機、搬貨==二種不同的狀態，再==判斷對應的狀態來控制手臂==。

---

## 範例程式

新增程式檔並命名 ==LAB04 - 控制二軸機械手臂==，將以下程式碼複製貼上程式編輯區執行。

```javascript=
window.state = "待機";

if(DATA.adc[0] == 1024){
  state = "待機";
}
if(DATA.adc[1] == 1024){
  state = "搬貨";
}
if(DATA.adc[2] == 1024){
  state = "停止";
}

console.log("手臂狀態： " + state);

if(state == "待機"){
  console.log("恢復至待機位置...");
  servo[0].set(120);
  delay(500);
  servo[2].set(180);
  delay(500);
  servo[4].set(30);
  delay(500);
  state = "停止";
  console.log("已恢復待機位置");
}
if(state == "搬貨"){
  console.log("搬貨中...");
  servo[4].set(90);
  delay(500);
  servo[2].set(120);
  delay(500);
  servo[0].set(30);
  delay(500);
  servo[2].set(180);
  delay(500);
  servo[4].set(30);
  delay(500);
  servo[2].set(120);
  delay(500);
  servo[0].set(120);
  delay(500);
  servo[2].set(180);
  delay(500);
  state = "停止";
  console.log("搬貨結束");
}
```

---

## 程式解說

逐行講解程式意義

---

```javascript=1
window.state = "待機";
```

- 以全域變數定義 state 的值為 "待機"。
- 作為判斷目前手臂狀態的值。

==在程式編輯區執行中只在第一次迴圈時執行==

---

```javascript=3
if(DATA.adc[0] == 1024){
  state = "待機";
}
if(DATA.adc[1] == 1024){
  state = "搬貨";
}
if(DATA.adc[2] == 1024){
  state = "停止";
}
```

- 分別判斷當不同按鈕按下時，改變成對應的狀態。

---

```javascript=13
console.log("手臂狀態： " + state);
```

- 顯示目前手臂狀態資訊。

---

```javascript=15
if(state == "待機"){
  console.log("恢復至待機...");
  servo[0].set(120);
  delay(500);
  servo[2].set(180);
  delay(500);
  servo[4].set(30);
  delay(500);
  state = "停止";
  console.log("已進入待機狀態");
}
```

- 當狀態為"待機"時，將第一個腳位的馬達設為 120 度，第三個腳位的馬達設為 180 度，第五個腳位的馬達設為 30 度，最後將狀態改變為"停止"，每個控制指令結束後需加上 delay。

---

```javascript=26
if(state == "搬貨"){
  console.log("搬貨中...");
  servo[5].set(90);
  delay(500);
  servo[2].set(120);
  delay(500);
  servo[0].set(30);
  delay(500);
  servo[2].set(180);
  delay(500);
  servo[5].set(30);
  delay(500);
  servo[2].set(120);
  delay(500);
  servo[0].set(120);
  delay(500);
  servo[2].set(180);
  delay(500);
  state = "停止";
  console.log("搬貨中...");
}
```

- 當狀態為"搬貨"時，配置的解釋同上。

---

## 參數修改

為方便實作，以下會將範例程式中可修改的參數標示出來，進行實作時只需修改對應參數，並觀察結果即可。

:::warning
:zap: 詳細內建 JS 參數參考 - [內建 Js 參數及功能總覽](/s/51F8nDF3Ss6M2DmGoRJQbA)
:::

---

:::success
**if (DATA.adc[==x==] == 1024)**
:::

- x 可代入 0 ~ 15，表示取得第 1 ~ 16 個腳位的 ADC 資料，依據使用的腳位填入對應的數字。

---

:::success
**servo[==x==].set(==y==);**
:::

- x 可代入 0 ~ 7，表示第 1 ~ 8 個伺服馬達輸出腳位，y 可代入 0 ~ 180，表示伺服馬達輸出的角度。

---

## 範例影片

{%youtube P-Dba6Zvjy4 %}

<a class="btn btn-warning" style="width:100%;color:#333333;"  href="/s/qQ3UtyWbRYiSbETukbvnlw" role="button"> 光敏電阻簡介與工作原理 **&#8680;** </a>

<a class="btn btn-primary" style="width:100%;"  href="/s/I1J0QniMRX66m652SJx1BA" role="button"> **&#8678;** PCA9685、I²C 與伺服馬達
</a>
