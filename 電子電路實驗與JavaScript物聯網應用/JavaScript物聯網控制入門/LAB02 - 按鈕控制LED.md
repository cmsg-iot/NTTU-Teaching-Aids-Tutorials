---
title: LAB02 - 按鈕控制LED
slideOptions:
  transition: slide
  transitionSpeed: "fast"
  theme: league
---

<!-- .slide: data-background="https://i.imgur.com/cgViIs1.jpg" data-background-color="#111111" data-background-opacity="0.2" -->

###### tags: `js-iot` `lab` 返回[電子電路實驗與 JavaScript 物聯網應用](/s/8Q89ww-EQlOg8zy7XSTlCw)

## LAB 02 <br> <span style="color:#F9BF45;">按鈕控制 LED</span>

###### [點我開啟簡報模式](/@BEExANT-ta/r1lioHRHt#)

###### <kbd>ESC</kbd> 鍵進入總覽模式

###### <kbd>&#8592;</kbd> <kbd>&#8593;</kbd> <kbd>&#8595;</kbd> <kbd>&#8594;</kbd> 切換頁面

---

## 目標

**在網頁上判斷按鈕腳位狀態並以 PWM 輸出控制 LED，按下時開啟、放開時關閉，觀察按鈕按下時網頁及 LED 的數據變化。**

---

## 使用材料

- **JavaScript 開發板**
- **220、1KΩ 電阻**
- **LED**
- **按鈕**
- **杜邦線**

---

## 線路圖

:::warning
:zap: 圖上麵包板正負極與實際不同，需注意正負極
:::

---

#### 按鈕控制 LED(220 電阻 _ 1、1K 電阻 _ 2)

<center> <img src="https://i.imgur.com/Ss9632V.png" width="75%"></img> </center>

---

## 設計原理

- 將按鈕接上 ADC 輸入，按下==按鈕接通時 ADC 為最大值==，表示已按下按鈕，在網頁上透過==程式判斷按鈕狀態來開/關 LED==。

---

## 範例程式

新增程式檔並命名 ==LAB02 - 按鈕控制 LED==，將以下程式碼複製貼上程式編輯區執行。

```javascript=
if (DATA.adc[0] == 1024)
{
    pwm[0].set(100);
}
else
{
    pwm[0].set(0);
}

delay(300);

if (DATA.adc[1] == 1024)
{
    pwm[1].set(100);
}
else
{
    pwm[1].set(0);
}

delay(300);

if (DATA.adc[2] == 1024)
{
    pwm[2].set(100);
}
else
{
    pwm[2].set(0);
}

delay(300);
```

---

## 程式解說

逐行講解程式意義。

---

```javascript=
if (DATA.adc[0] == 1024)
{
    pwm[0].set(100);
}
else
{
    pwm[0].set(0);
}
```

- 判斷第一個==ADC 輸入值是否為 1024== (1024 為最大值，表示按鈕按下導通電路)。
- 為==真==則將第一個==PWM 輸出設為 100==。
- 為==假==則將第一個==PWM 輸出設為 0==。

---

```javascript=
delay(300);
```

- 每次使用控制命令後需等待至少==200~300 毫==秒後再繼續執行。

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
**pwm[==x==].set(==y==);**
:::

- x 可代入 0 ~ 7，表示第 1 ~ 8 個 PWM 輸出腳位，y 可代入 0 ~ 100，表示 PWM 輸出的大小。

---

## 範例影片

{%youtube 9xaD6uuBWA4 %}

<a class="btn btn-warning" style="width:100%;color:#333333;"  href="/s/iBzaKT_rQb2OzkyF3FJLwA" role="button"> PWM 與電晶體 **&#8680;** </a>

<a class="btn btn-primary" style="width:100%;"  href="/s/4CgYHd9BREiq0TwqEWpRKw" role="button"> **&#8678;** JavaScript 基礎語法與變數運用
</a>
