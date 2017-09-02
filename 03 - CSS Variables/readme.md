# 03 - CSS Variables

## Effect
實作可藉由調整控制條，即時調整圖片的邊框(padding)、模糊程度、與邊框背景顏色的效果

[玩玩看~](https://arcobalenoi27.github.io/javascript30/03%20-%20CSS%20Variables/index-START.html)

## Points
1. :root
2. CSS Variable
3. 模糊效果 filter: blur()
4. 藉由javascript改變CSS variable的值


## Preparation
### CSS part
 - 在:root裡面宣告CSS variable
 - 把會改變的style效果，用CSS variable來處理

### Javascript part
 - 取得3個input的DOM element
 - 監聽每個input的變化 - mousemove事件(滑鼠滑過該element時觸發的事件)、change事件
 - 依據監聽到的事件來更新CSS variable
  + 獲取更新後的數值
  + 獲取更新的項目名稱(blur、spacing、base)
  + 更新相應的CSS variable

## How it works(Implement)
1. 如何動態改變圖片的各種style?  在這個範例我們會使用CSS variable來對應各個style的效果(邊框尺寸、邊框顏色、模糊度)，我們把CSS variable對應到每個style效果的值，再藉由javascript動態改變這些CSS varibale的值。

2. 模糊效果: 使用CSS的filter 屬性

  

## Detail(Syntax)
1. filter屬性: 這是一個CSS提供的屬性，可用來做出模糊、圖形特效、銳利化、元素變色、色彩亮度飽和度調整等等效果，類似於影像處理軟體的效果，使用時，直接呼叫它所提供的函數就能得到效果了
 - [各瀏覽器支援度](http://caniuse.com/#search=filter)

2. CSS variable / CSS變數 - 1種CSS3的新屬性，宣告的命名原則是`--變數名`，使用這個變數時的語法為`var(--變數名)`
```css
    :root {
        --color: #00e0ff;
        background-color: var(--color);
    }
```
3. dataset屬性(HTML 5) - 在HTML5裡面，我們可以為DOM element加上自訂的屬性，用法是以`data-`為前綴的屬性名稱，-後面接你想命名的變數名稱，如: data-unit，然後我們可以用dataset來取得這些自己的屬性值(任何`data-`開頭的都可以)，dataset是一個object(DOMStringMap)，裡面包含在html裡面預先自訂好的各屬性，取得DOM屬性的值的方法跟取得object內部屬性的方法一樣，使用dot(.)
```html
    <input id="blur" type="range" name="blur" min="0" max="25" value="10" data-sizing="px">
```
```javascript
    let input = document.querySelector('#blur');
    input.addEventListener('change', function () {
         let sizing = this.dataset.sizing;
    });
```

4. `:root` psudo-class 這個psudo class對應到html文件的root element　－　html tag `<html>`，通常我們會在這裡宣告CSS variable
```css
    :root {
        --color: #00e0ff;
    }
    img {
        background-color: var(--color);
    }
```

## Thinking
1. 如何使用javascript改變CSS variable的值?
 + Javascript裡面的`document.documentElement`會return html文件的root元素 - `<html>` html 標籤，所以我們就可以藉root元素style的setProperty這個方法來更新CSS variable的值:
```javascript
    document.documentElement.setProperty('--color', '#00e1ff');
```

2. 如何處理各個style效果裡面的CSS屬性值的單位(px)?
 + 我們可以先在html先預訂好`data-變數名`的dataset資料，如: 
```html
    <input id="spacing" type="range" name="spacing" min="10" max="200" value="10" data-sizing="px">
```
+ 在用javascript裡面的`dataset.變數名`(dot)來取得這些CSS單位，在這次這個例子裡面記得顏色方面的CSS並沒有單位，javascript會獲取不到data-sizing的值，也就是說會拿到`undefined`，所以記得用 or邏輯寫個預設值
```javascript
    const suffix = this.dataset.sizing || '';
```
