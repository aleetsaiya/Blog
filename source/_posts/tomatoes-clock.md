---
title: "Tomatoes Clock 製作心得記錄"
date: 2021-07-07 00:16:46
---

## 前言
因為在網路上看到 [The F2E](https://challenge.thef2e.com/) 的前端作品訓練，其中第一關是 Tomatoes clock 想說我也來做做看好了，雖然沒有版上大家練習做得那麼好看，但是就當作是給自己的一個挑戰順便練習一下 javascript。

## Tomatoes Clock 介紹
Tomatoes Clock 就是我們花 25 分鐘專注在自己設定的事物上，然後給休息 5 分鐘，這樣可以讓我們一方面提升自己的工作效率，一方面讓我們擁有足夠的休息時間。而除了將時間分割成 25, 5 分鐘以外，也要先設定好自己在做一個事情可能會花費的番茄數，做完了之後再回頭看自己有沒有跟當初預期的番茄數差不多，有沒有在做的過程中浪費時間在其他瑣碎的事情上。

## 製作過程
那接下來就紀錄自己在做這個網頁版的番茄時鐘遇到的問題以及心得吧。

作品網址 : [Tomatoes clock](https://aleetsaiya.github.io/clock/#)
程式碼: [Github](https://github.com/aleetsaiya/PotatoesClock)

### CSS
因為練習量的不足，所以在 css 排版上花了很多時間( 雖然我做的排版已經很簡單了，還是花很多時間在一些基礎的東西上 ... )，然後這是自己第一次套用 bootstrap 搭配自己的做的作品，所以很多命名好像都跟 bootstrap 的命名方式衝突，花了一大筆時間在看為什麼自己的網頁會長成這樣，之後可能要多練習切版以及在套用 bootstrap 時的一些操作。

而 navbar 切換不同 page 的動畫效果，我原本想說可以做那種 *"page 在右邊就從右邊滑進來，在左邊就從左邊滑近來"* 的那種感覺，但可能是因為自己 css 不夠純熟所以雖然一開始是做出來了，可是後來排版一直跑掉而且還要加新的 "Record" 頁面，所以後來就放棄改成全部都從左邊滑近來。雖然感覺效果也是很好啦但還是帶著一點小遺憾。最後就是覺得在不同裝置上都要顯示的好看真的很難 ... 。

### Javascript
因為之前有在 javascript30 使用到 Event Delegate，所以這次回想了一下然後就在 todo-list 上使用。概念就是比起把每一個子節點都綁定事件，直接在父節點綁定事件，然後根據 `e.target` 來去偵測它是誰。

這次也自己實作了之前在 javascript30 上學習到的 **localStorage**，透過 localStorage 紀錄資料讓我們下一次進入同一個網址或著是重新整理，能夠獲得之前的資料。我這邊是利用 localStorage 來紀錄使用者的待作清單。

然後原本覺得很簡單很快就能做完的時間倒數部分，單純的時間倒數倒是很快就做好了，可是配合 stop, start, 以及切換模式後就發現很多細節自己都沒有設定到導致很多 bug ，在這邊也是花了比自己預期還要久的時間。

## 結語
這次會做這個番茄終的想法真的很突如其來，不知道為什麼就開始做了的那種感覺，自己在做完之後也複習以及學到了一些新的知識，感覺還不錯！重點是自己做出來一個能看的東西真的很爽 ~ 

-----------------------------------------------------
*2021/7/10 更新*

後來在 javascript30 也有做到 *Count down timer* 。看完了之後發現我之前的寫法真的級醜無比，用了一大堆 global variable 然後命名也命名的很隨便，所以後來重新把 js 中的一些部分做了修改，盡量的讓自己的程式碼比之前的更好維護，然後也更有架構以及邏輯性。

## 減少 Global variable

要減少 Global variable 一開始還真的完全沒概念，所以上網查了一點資料。找到了這篇 [JavaScript best practices](https://www.w3.org/wiki/JavaScript_best_practices)，其中的 Avoid Global 單元中介紹到 **Module Pattern** ，大概就是使用 [IIFE (Immediately Invoked Function Expression)](https://developer.mozilla.org/zh-TW/docs/Glossary/IIFE) 將變數限制在 function 範圍內，讓外部不允許使用內部的變數。而我們也可以自己設定要 `return` 的值 (讓外部使用的值)。

ex. 程式碼內 tomatoes 部分
```javascript
const tomatoes = (function() {
    let tomatoesNumber;
    function init() {
        tomatoesNumber = parseInt(localStorage.getItem('tomatoesNumber')) || 0;
        setTomatoes(tomatoesNumber);
    }

    function getTomatoes() {
        return tomatoesNumber;
    }

    function setTomatoes(number) {
        const tomatoesDisplay = document.querySelector('.tomatoes');
        tomatoesNumber = number;
        localStorage.setItem("tomatoesNumber", tomatoesNumber);
        tomatoesDisplay.innerHTML = `Total: 🍅 x ${tomatoesNumber}`;
    }

    return {
        init: init,
        getTomatoes: getTomatoes,
        setTomatoes: setTomatoes
    }
})();
```

外部只能允許使用 `return` 內的 `init`, `getTomatoes` 以及 `setTomatoes`，而 `tomatoesNumber` 則沒辦法在外部使用。

## 讓程式碼更好維護
除了更新了一些原本亂命名的變數外，也盡量讓 function 的命名更容易看得懂，之後回過頭來看至少不會不知道自己在寫什麼東西。而因為剛看完 `Mosh` 的課程教學，裡面有一個部分是要存所有 error 情況，我記得他是用 `list` 裡面裝 `object` 並把一些錯誤訊息也放進去，所以這次也試著寫了差不多概念的。

```javascript
// Error Check
const errors = [
    {validate: isStudy && name === 'study', message: '⚠️ "Study" is the current mode.'},
    {validate: isRest && name === 'rest', message: `⚠️ "Rest" is the current mode.`},
    {validate: leftTime === 0 && name === 'playpause', message: '⚠️ Change the mode to keep going !'}
];
const error = errors.find(error => error.validate ? error.message : false);
if (error){
    toastify.options.text = error.message;
    toastify.showToast();
    return;
}
```

像這樣子寫就比之前原本寫的還要清楚，錯誤的情況有哪些，相對應的錯誤訊息也很容易看出來，要增加錯誤情型也很好增加，而其它的像是有些設定可以寫在 html 的 `data-*` 裡面。之前原本不同 mode 的時間設定我是寫在 javascript 裡面，可是發現寫成 `data-name="study"` 放在 html 裡面就好了，javascript 可以直接用 `dataset.name` 裡面將值取出來。

## 總結
使用 IIFE 來避免 Global variable 我真心覺得超舒服，跟之前的程式碼相比差了很多，這次只是第一次用之後再把相關的概念補足起來。