# 01 JavaScript Drum Kit 

## Effect
實作1個打鼓頁面，當User按下A、S、D、F、G、H、J、K、L，這幾個鍵時，頁面上的各個對應的字母的按鈕會有特效，且播放對應的鼓聲。

[玩玩看~](https://arcobalenoi27.github.io/javascript30/01-JavaScript%20Drum%20Kit/index-START.html)

## Points
1. 鍵盤事件
2. 特效顯示
3. 播放聲音檔案

## How it works(Implement)
1. 在window監聽鍵盤的事件(keydown)
2. 依據監聽到的按鍵去做對應的事情，如下:
    + 用querySelect和CSS Select(data-key)，依據keyCode去獲取對應的audio、div element
    + 實作頁面效果(播放音樂-鼓聲、在對應的element上加上CSS transition效果)

3. 用querySelectorAll去獲取所有頁面上的element，並獲取個別按鍵對應的element(key)，監聽這些CSS transtiion效果結束的事件，並加上效果結束時，該執行的函數(handler)-把對應的element的CSS class去除掉(playing)

## Detail(Syntax)
### ES6語法
    1. const: 用const宣告變數只能assign 1次
    2. `I'm ${變數名稱}`: 這個語法叫做Template Literals(TL)，用法的字首跟字尾的字元都是‵(位於tab鍵的上方)，這個語法可以幫我們解決一些字串合併、串接的問題，ES6以前開發者常常會遇到字串要串接變數的情況，寫法會變得很麻煩，案例如下:
    ```javascript
        var doge = 'Doge';
        var race = 'Shiba Inu';
        // 不用TL的寫法
        console.log('名字: ' + doge + ', 品種: ' + race );
        // TL寫法，簡潔許多
        console.log(`名字: ${doge}, 品種: ${race}`);
    ```
    3. querySelect、querySelectorAll，前者return一個element，後者return裡面包含所有符合CSS選擇器或條件的element的Array
    4. => (Arrow Function / 箭頭函數)，在forEach裡面，對群體中的個別的element做事件綁定時，通常會放1個callback函數，這時候可以用這個語法來寫，優點是較簡潔，且當this對象不同的時候，不用自己用bind()來綁定，箭頭函數會自動幫你綁定

## Thinking
    I. 要怎麼把window監聽到的按鍵跟聲音的audio元素與網頁的div元素對應起來?
        + 利用在html標籤裡面加入data-key的attribute的值來對應按鍵的keyCode，再用querySelect去抓到對應的元素，這樣一來，就可以做出對應的動作(播放音效、展示按鍵按下效果)
    II. 要怎麼讓按下按下時就可以即時播放出音效，而不用等前一個音效播放完，也就是聲音會連續播放
        + 在每次播放音效前，都先把播放時間設成 0
        ```javascript
            let audio = document.querySelect('audio');
            audio.currentTime = 0;
            audio.play();
        ```
    III. 要怎麼偵測頁面效果已經執行完畢，然後去除播放時在元素上面加的class等style ?
        + 可以用一個叫做`transitionend`的事件來做判斷，當CSS transition執行完的時候這個事件會被觸發，這時我們就可以用這事件來做解除元素上面的class的樣式效果，這裡不能用toggle方法，使用toggle的話，元素會一直閃爍播放音樂時的黃色外框效果