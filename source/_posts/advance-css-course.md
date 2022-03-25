---
title: Advance CSS and Sass
date: 2021-07-18 00:05:44
---
> _Advance CSS and Sass: [https://www.udemy.com/course/advanced-css-and-sass](https://www.udemy.com/course/advanced-css-and-sass)_

紀錄於 Udemy 上由 _Jonas Schmedtmann_ 推出的 _Advanced CSS and Sass: Flexbox, Grid, Animations and More!_ 課程筆記。


## Note
### em, rem, px
+ `em` 是根據 parent 的 `font-size` 來去做計算，而 `rem` 是根據 root 的 `font-size` 來去做計算(使用 em 或是 rem 能夠讓我們更容易操作 RWD )。
+ 因為有些使用者會自行調整頁面的字體大小，所以當我們在設定 `rem` 的 root font-size 時，比起使用固定的 `px`，我們可以改以 % 來去代替(因為知道 webpage default font-size 為 16px 所以可以很容易計算出我們要指定的大小)。
+ 瀏覽器的初始 `font-size` 為 _16px_ 。

### BEM
Get BEM: [http://getbem.com/naming/](http://getbem.com/naming/)
全名為 _Block Element Modifier_，為一種命名的方式。規則如下 : 
```css
.block{}
.block__element {}
.block__element--modifier {}
```

### Box Model
> _box-sizing: border-box 的意思為 total_width, total_height == specified_
+ content
+ pading
+ border
+ margin

### Three pillars to write good html and css
+ Responsive design
+ Maintainable and scalable code
+ Web performance

### What happen when we load up a webpage ? 
網站會先 load HTML，然後 parse HTML 讓我們獲得 _Document Object Model_ (DOM)。而 css 也一樣，我們會 load CSS，然後 parse CSS (包含 resolve conflict, process final css value ex. 50%) 使我們獲得 _CSS Object Model_ (CSSOM)。獲得 DOM 以及 CSSOM 之後我們就可以 render tree，產生我們看到的網站。

### resolve conflict
會依照下列順序來去比對哪一個優先權種比較高 : 
1. `!important`
2. inline-style > ids > classes > elements
3. source order (所以自己 custom 的 css 檔最好放在最底下)

## Sass
Syntax - Sass : [https://sass-lang.com/documentation/syntax](https://sass-lang.com/documentation/syntax)

語法格式分為 _Sass syntax_ 以及 _SCSS syntax_ (Sassy CSS)，前者是用縮排來排版，後者是用 `{}` 來排版。

### variable
設定變數的方法是在變數名稱前加一個 `$`。
```scss
$color-primary: #f9ed69; //yellow
nav {
    background-color: $color-primary;
}
```
### nested
範例中 pseudo class 前面的 `&` 代表 `li` 後面直接接 `:first-child`，中間沒有空格。
```scss
.navigation {
  list-style: none;
  
  li {
    display: inline-block;
    margin-left: 30px;
    
    &:first-child {
      margin: 0;
    }
  }
}
```

### @mixin
有點像是 function 那樣把常用到的程式碼寫到一個 mixin 裡面，然後進行重複使用。
```scss
// 設定 mixin
@mixin style-link-text($color) {
    text-decoration: none;
    text-transform: uppercase;
    color: $color;
}

.link-text:link {
    @include style-link-text($color-text-dark); // 使用 style-link-text
}
```


### @function
```scss
@function divide($a, $b) {
    @return $a / $b;
}

nav {
    margin: divide(60, 2) * 1px; //30px
}
```


## Natours
>於 Natours Project 內學習到的知識

### clip-path
將 block 裁切成特殊形狀，依照順時針方向設定4個點的 x, y 座標。
Clippy — CSS clip-path maker: [https://bennettfeely.com/clippy/](https://bennettfeely.com/clippy/)

```css
.block{
    /* 順時針方向設定 4 個點 x, y 座標 */
    /* 左上 右上 右下 左下 */
    clip-path: polygon(0 0, 100% 0, 100% 75%, 0 100%);
}
```

### animation
製作 animation 很簡單，先設定一個 `keyframes` 並指定它的名稱以及不同 % 數的變化，接下來就直接套用在 block 上就好了

```css
.block {
    animation: moveInRight 1s ease-out;
}

@keyframes moveInRight {
    0% {
        opacity: 0;
        transform: translateX(100px);
    }
    
    80% {
        transform: translateX(-10px);
    }
    
    100% {
        opacity: 1;
        transform: translateX(0);
    }
}
```

### button 點擊後有按下去的感覺
為了讓 button 點下去後有按下去的感覺，我們要操縱 `box-shadow` 以及 `transform`。如果 hover 的話就顯示 box-shadow 並把 button 往上移一點點，然後如果點擊的話就把剛剛的 box-shadow 中 y-axis 設定小一點並把 button 往下移一點讓我們有離網頁更近的感覺。

```css
.btn:hover {
    transform: translateY(-3%);
    box-shadow: 0 10px 20px rgba(0, 0, 0, .2);
}

.btn:active {
    transform: translateY(-1px);
    box-shadow: 0 5px 10px rgba(0, 0, 0, .2);
}
```

### animation-fill-mode
決定動畫在播放前(等待階段)或是之後(動畫結束)，動畫效果是否要套用在元素上。
```css
.block {
    animation: moveInBottom .5s ease-out .75s;
    /* 在 delay 的 .75秒時(播放前)，使用動畫 0% 效果 */
    animation-fill-mode: backwards;
}
```