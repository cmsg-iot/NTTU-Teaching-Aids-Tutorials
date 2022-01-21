###### tags: `js-iot` 返回[電子電路實驗與 JavaScript 物聯網應用](/s/8Q89ww-EQlOg8zy7XSTlCw)

# JavaScript 基礎語法與變數運用

**JavaScript 常用於網頁設計，例如網頁上按的按鈕、選單、帳號密碼輸入視窗等等互動元件，有豐富的學習資源及語法簡易上手，與其他程式語言相比較容易實現想法。**

**在教具中使用 JavaScript 進行運算、邏輯判斷與控制，以下將簡單介紹幾種 JavaScript 的型別與基礎語法，之後在每個 LAB 的實作中會再更進一步說明。**

## 資料型別

:::success

- JavaScript 是弱型別，也能說是動態的程式語言。
- 不必特別宣告變數的型別。
- 程式在運作時，型別會自動轉換。
- 可以以不同的型別使用同一個變數。
- 撰寫 JS 程式時，結尾不一定要加分號 ; ，JS 的特性在執行會自動補上，但習慣上還是建議在每行程式結束時要加上分號。
  資料來源 - [JavaScript 的資料型別與資料結構](https://developer.mozilla.org/zh-TW/docs/Web/JavaScript/Data_structures)
  :::

:::info
:bulb: 電腦操作下可按 <kbd>F12</kbd> 開啟開發者工具，在 Console(主控台)中輸入以下內容進行練習吧！
:::

### 數字型別(number)

:::warning
:zap: JavaScript 數字型別的只有一種，能代表正負整數、浮點數，詳細參考[數字型別](https://developer.mozilla.org/zh-TW/docs/Web/JavaScript/Data_structures#%E6%95%B8%E5%AD%97%E5%9E%8B%E5%88%A5)
:::

```javascript=
123 + 456;
// 相加，結果爲 579

123 - 456;
// 相減，結果爲 -333

111 * 6;
// 相乘，結果為 666

100 / 3;
// 相除，結果為 33.333333333333336

100 % 3;
// 取餘數，結果為 1

5 + 5 * 3;
// 先乘除後加減，結果為 20

(5 + 5) * 3;
// 括號內優先運算，結果為 30
```

:::info
:globe_with_meridians: 更多範例參考

- [W3schools - JavaScript Numbers](https://www.w3schools.com/js/js_numbers.asp)
  :::

### 字串型別(string)

:::warning
:zap: 有兩種方法可以表示字串，由成對的"或'引號所包起來的字就變成字串，如 "123"、'123'。
:::

```javascript=
"100" + "10";
// 字串相加，結果為 "10010"

"100" + 10;
// 字串+數字會統一變字串，結果為 "10010"

10 + "100";
// 數字+字串反過來一樣都變字串，結果為 "10100"

"100" / "10";
// 但遇到+號以外的運算時，會以數字行別下去運算，結果為 10

"C8763".length;
// string型別中的length方法可以得知字串的長度，結果為 5
```

:::info
:globe_with_meridians: 更多範例參考

- [W3schools - JavaScript Strings](https://www.w3schools.com/js/js_strings.asp)
  :::

### 布林型別(boolean)

:::warning
:zap: 布林（Boolean）代表了有兩個值的邏輯實體：true(真) 與 false(假)。
:::

```javascript=
1 < 2;
// 1是否小於2，結果為 true

1 > 2;
// 1是否大於2，結果為 false

1 == "1";
// ==(寬鬆相等)在JS中會將兩邊轉換成相同型別後再進行比較，結果為 true

1 === "1";
// ===(嚴格相等)在JS中會先比較兩邊型別是否相同，結果為 false

1 != "1";
// !=(寬鬆不相等) 為==的反例，結果為 false

1 !== "1";
// !==(寬鬆不相等) 為===的反例，結果為 true

!(1 < 2);
// 結果為 false
// ! 將布林結果進行not運算，true變false，false變true

(1 < 2) && (3 > 2);
// 結果為 true
// && 將兩邊布林結果進行and(且)運算，兩邊都為true則結果為true，一邊為false則結果為false

(1 < 2) || (3 < 2);
// 結果為 true
// || 將兩邊布林結果進行or(或)運算，只要一邊有true則結果為true，兩邊為flase則結果為false
```

:::info
:globe_with_meridians: 更多範例參考

- [W3schools - JavaScript Booleans](https://www.w3schools.com/js/js_booleans.asp)
  :::

### 物件型別

:::warning
:zap:
obj = {key1:value1,key2:value2};
使用{}定義的物件，可以透過點記號法 (obj.key1)，或是方括號記號法 (obj['key1'])來存取數值。

array = [value1,value2];
使用[]定義的物件(又稱陣列)，只能透過方括號指定第幾個元素(array[0])來存取數值，在程式的陣列中以 0 作為第一個元素值。
:::

```javascript=
obj ={
   a: 123,
   b: '123',
   "布 林 值": true,
   array:[1,2,3],
   func:function(){console.log("Hello,world!")}
};

obj.a;
obj.b;
// 可以用 obj.a 取得 123，obj.b 取得 '123'

obj["布 林 值"];
// 但遇到特殊資源作為key時，需用obj["布 林 值"]才可以取值

obj.array[0];
// 也可以將陣列放在物件裡，使用obj.array[0]可以取得array陣列中第一個元素的值

obj.func();
// 還可以將函式放裡面，使用obj.func()可呼叫裡面的函式，也可以將數值代入給函式
// 在LAB將會以這種方式進行各種設定與控制

console.log(obj);
// 可以使用console.log查看obj物件所包含的內容

```

### typeof 取得資料型別

:::warning
:zap: typeof 運算子會傳回一個字串值, 指出未經運算 (unevaluated) 的運算元所代表的型別。
:::

```javascript=
// Numbers
typeof 123;
typeof 3.14;
// 結果為 'number'

// Strings
typeof "";
typeof "你好";
// 結果為 'string'

// Booleans
typeof true;
typeof false;
// 結果為 'boolean'

// Undefined
typeof undefined;
// 結果為 'undefined'

// Objects
typeof {a:1};
typeof [1, 2, 4];
// 結果為 'object'

// Functions
typeof function(){};
// 結果為 'function'
```

:::info
:globe_with_meridians: 更多範例參考
<br>

- [相等比較](https://developer.mozilla.org/zh-TW/docs/Web/JavaScript/Equality_comparisons_and_sameness)
- [W3schools - JavaScript Booleans](https://www.w3schools.com/js/js_booleans.asp)
- [W3schools - JavaScript Comparison and Logical Operators](https://www.w3schools.com/js/js_comparisons.asp)
- [MDN Web Docs - typeof](https://developer.mozilla.org/zh-TW/docs/Web/JavaScript/Reference/Operators/typeof)
  :::

## 運算子

:::success
在 JavaScript 中有多種運算子，包括賦值運算子，比較運算子，算術運算子，位元運算子， 邏輯運算子, 字串運算子, 條件(三元)運算子 以及更多運算子。

建議參考 [JavaScript 運算式與運算子](https://developer.mozilla.org/zh-TW/docs/Web/JavaScript/Guide/Expressions_and_Operators) 瞭解更多運算子應用。
:::

## 變數

:::success

- 變數（variable）是對值（value）的引用
- 變數必須使用字母（letter）、下底線（ \_）、錢號（$）作為開頭
- 後面的字員組成可以包含數字（0-9）
- 變數區分大小寫（case secsitive）
  資料來源 - [語法與型別](https://developer.mozilla.org/zh-TW/docs/Web/JavaScript/Guide/Grammar_and_types)
  :::

### 定義變數

:::warning
:zap: 常見宣告變數會使用 var、let，分別代表不同變數範圍的宣告
:::

```javascript=
var a = 101;
// 宣告一個變數 a ，裡面的值為數字101

let b = "大樓";
// 宣告一個變數 b ，裡面的值為字串

c = a + b;
// 意思是 將c設為 a+b
// 也可以不加var、let直接定義變數，等同於定義window變數，c 為 "101大樓"
```

### 變數取值

:::warning
:zap:

- 變數可以透過 var 或是 let 來定義，如果尚未指定數值給該變數，那麼該變數的值會是 undefined。
- 如果嘗試去存取未定義的變數，會跳出 ReferenceError 的例外。
  :::

```javascript=
var a;
console.log("a 的值為 " + a);
// 未指定數值給a，使用console.log輸出訊息結果為 a的值為 undefined

console.log(b);
// b未宣告，會跳出 ReferenceError: b is not defined 的錯誤訊息

console.log(c);
var c;
// var宣告變數放在後面，即使未指定數值也會得到 undefined 的結果

console.log(d);
let d;
// let宣告變數必須在前面，否則會得到 ReferenceError: d is not defined 的錯誤訊息

```

### 變數範圍

:::warning
:zap:

- 在函式外宣告一個變數時，這個變數會是一個全域變數 (global variable)，在這份程式文件裡面的所有程式碼都可以使用到這個變數。
- 在函式內宣告變數時，這變數是區域變數 (local variable)，變數只會在函式內被使用到。
  :::

```javascript=
if(true)
{
  var x = 5;
}
console.log(x);
// x 為 5
// 雖然 x 是在 if { } 區塊裡面被宣告的，但卻因為有全域變數的特性，因此溢出大括號而成為後續描述碼的變數值。

if (true) {
  let y = 5;
}
console.log(y);
// ReferenceError: y is not defined (y沒有被定義)
// 當使用了 let  這個區域變數宣告方式，變數 y 的有效範圍只有在 if { } 的範圍內，因此輸出結果是 ReferenceError。
```

## 內建函式

:::success
以下介紹幾個在實作中會使用到的內建函式。
:::

### console.log

:::warning
:zap: 輸出變數或運算的訊息，常用於查看變數在經過運算後的結果
:::

```javascript=
console.log("Hello,world!");
// 直接輸出字串訊息，結果為 Hello,world!

console.log(123+456);
// 也可以將運算結果輸出，結果為 579

a = 111;
b = 222;
console.log(a+b);
// 將變數運算後輸出，結果為 333
```

### if...else

:::warning
:zap:

if(條件式)
{statement1}
else
{statement2}

中括號放置條件式(布林型別)，當判斷結果為 true(真)時執行 statement1 中的程式，為 false(假)時執行 statement2 中的程式。
:::

```javascript=
if(true)
{
    console.log("判斷爲 真");
}
else
{
    console.log("判斷為 假")
}
// 結果為 判斷爲 真

a = 111;
b = 222;
c = 333;

if((a+b) > c)
{
    console.log("a加b 大於 c");
}
else
{
    console.log("a加b 不大於 c");
}
// 結果為 a加b 不大於 c，嘗試將數值改變看看結果吧!

a = 222;
b = 111;
c = 333;

if((a < b) && (b < c))
{
    console.log("a小於b 且 b小於c");
}
else if((a > b) || (b > c))
{
    console.log("a大於b 或 b大於c");
}
else
{
    console.log("以上兩種條件都不成立");
}
// 若有多個條件判斷，在if與else間使用else if可以加入多個判斷，結果為 a大於b 或 b大於c

a = "事件一";

if(a === "事件一")
{
    console.log("觸發事件一");
}
else if(a === "事件二")
{
    console.log("觸發事件二");
}
else
{
    console.log("例外事件");
}
// 利用字串定義事件進行判斷，可以改變變數中的字串切換觸發事件，結果為 觸發事件一

```

:::info
:globe_with_meridians: 更多範例參考

- [Javascript console.log()用法及代碼示例](https://vimsky.com/zh-tw/examples/usage/javascript-console-log-with-examples.html)
- [MDN Web Docs - if...else](https://developer.mozilla.org/zh-TW/docs/Web/JavaScript/Reference/Statements/if...else)
  :::

:::info
:+1: 如果想瞭解更多 JavaScript，可以參考下列文章

- [重新認識 JavaScript](https://ithelp.ithome.com.tw/users/20065504/ironman/1259)
- [你懂 JavaScript 嗎？#2 暖身 (๑•̀ㅂ•́)و✧ Part 1 - 運算子、運算式、值與型別、變數、條件式、迴圈](https://cythilya.github.io/2018/10/09/intro-1/)
  :::

<a class="btn btn-warning" style="width:100%;color:#333333;"  href="/s/ViJQSKkDTgKc0X_is4fOAQ" role="button"> LAB02 - 按鈕控制 LED **&#8680;** </a>

<a class="btn btn-primary" style="width:100%;"  href="/s/qKRu3CJqTb6RpQ6I1Z_jwA" role="button"> **&#8678;** LAB01 - ADC 電壓量測實驗
</a>
