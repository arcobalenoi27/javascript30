# 02 CSS + JS Clock

## Effect
實作1個即時更新的時鐘，原始的HTML結構已經畫好基本的時針跟時鐘的形狀，我們所要做的是動態更新指針的位置。

[玩玩看~](https://arcobalenoi27.github.io/javascript30/02-JS%20and%20CSS%20Clock/index-START.html)

## Points
1. CSS旋轉效果
2. 取得現在時間與計算指針角度
3. 每秒更新指針狀態

## How it works(Implement)
1. 這次時鐘會使用到以下的CSS屬性:
```css
    .second-hand {
        transform: rotate(90deg);
        transform-origin: 100%;
        transition: all 0.05s;
        transition-timing-function: cubic-bezier(0.59, 1.75, 1, 1);
    }
```
  + 我們需要用到transform-origin來設定指針的旋轉圓點
  + transform: rotate()用來更新我們的指針的角度
  + transition是當每次指針角度變化時，我們會在上面加上過渡效果，好讓秒與秒之間的變化，看起來不那麼劇烈且不協調
  + transition-timing-function是設定過渡效果(在這裡是指針跳轉的過渡效果)的隨時間經過，欲改變數值與時間的關係與程度，此屬性可用chrome dev tools或者貝茲曲線產生器(bezier generator)來模擬比較容易產生自己所需要的函數

2. 利用Date()來獲取現在此時刻的時間數值，並依據數值計算各指針對應角度:
    + 秒針的角度 = 秒數 /60 * 360 °
    + 分針角度 = 分鐘數/ 60 * 360  + 秒數 / 3600 * 360 °
    + 時針角度 = 小時數(12小時制) / 12 * 360 + 分鐘數 / 60 / 12 * 360 + 秒數 / 12 / 3600 * 360 °

3. 利用setInterval函數來動態每秒更新指針位置
  

## Detail(Syntax)
### Date Object and setInterval
1. 使用Date Object獲取現在時刻，並利用這個object的method來獲取當下時間的各數值(時、分、秒)
```javascript
    const date = new Date();
    const seconds = date.getSeconds();
    const minutes = date.getMinutes();
    const hours = date.getHours() % 12;
```
2. setInterval作用為每秒執行某個函數，在此用於每秒動態更新時間(指針位置)
```javascript
    setInterval(setDate, 1000);
    // 在javascript是以毫秒計算,  所以每秒更新就是每1000毫秒更新1次
```

## Thinking
1. 當指針轉了一圈，從59秒(分) → 0秒(分)或11時 → 12時(0時)的時候，由於原先設定指針跳轉的過渡時間是0.05秒，所以會在指針歸零前到歸零時，畫面會閃爍一下，如果我們設定的過渡效果時間更長，像是1秒，指針就會從444°(360*59/60 + 90)逆時針轉到90°後，再繼續順時針轉動，這並不是我們正常鬧鐘所預期的順時鐘轉動，在這邊用一個函數來確認指針是否回歸到12點鐘方向，並把過渡效果的時間設定為0，就可以避免指針逆時針轉動的現象，並依照我們的期望順時針轉動。
