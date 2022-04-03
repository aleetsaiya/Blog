---
title: Style your windows powershell and git bash
date: 2022-04-02 22:45:47
tags:
---

之前一直看到別人將自己的 terminal 改的很酷炫，可以顯示 git branch 而且還有一大堆顏色，今天有時間研究了一下終於也把自己的 terminal 增加了這些功能，順便記錄一下操作過程以免之後換電腦不知道要怎麼用。

完成後的模樣:

<img width="725" alt="螢幕擷取畫面 2022-04-03 145841" src="https://user-images.githubusercontent.com/67775387/161415750-1b5e7d01-2391-4450-a482-57a00830f66f.png">  

## Windows PowerShell
基本上如果是要在 windows powershell 上有這些功能，只要跟著官方提供的方式 ([教學課程：使用喔 My Posh 設定 PowerShell 或 WSL 的自訂提示](https://docs.microsoft.com/zh-tw/windows/terminal/tutorials/custom-prompt-setup#customize-your-powershell-prompt-with-oh-my-posh) 就可以完成了，大致步驟如下:

1. 從 [NerdFonts](https://www.nerdfonts.com/font-downloads) 安裝想要的字形 (為了避免有些 icon 跑不出來出現白框框)，解壓縮後應該會有 `True Type font file` 的檔案，將這些檔案丟到 *控制台* -> *外觀及個人化* -> *字形* 裡面
2. 從 windows terminal 的設定中，點擊 *powershell* -> *外觀*，將文字改成下載的字體
3. 在 terminal 上輸入
```powershell
Install-Module oh-my-posh -Scope CurrentUser
```
下載 oh-my-posh 以及其主題
4. 從 [oh-my-posh themes](https://ohmyposh.dev/docs/themes) 上挑自己想要的主題
5. 在 terminal 上輸入
```powershell
code $PROFILE
```
開啟 vscode 設定我們的 PowerShell 設定檔
6. 在開啟的檔案中新增程式碼:
```powerShell
Import-Module oh-my-posh
Set-PoshPrompt -Theme paradox
```
paradox 是主題的名稱，也可以換成其它自己想要的主題

> PowerShell 設定檔是在 PowerShell 啟動時執行的指令碼。您可以使用設定檔做為登入腳本來自訂環境。
 
接下來重新開啟 terminal 就可以有新的主題了!

### Git bash
我自己是在 windows terminal 上使用 git bash，方法可以參考 [如何在 window terminal 增加 git bash](https://aleetsaiya.github.io/2022/01/20/window-terminal-%E5%A2%9E%E5%8A%A0-git-bash/)。

1. 跟上述第一步一樣安裝字體
2. 從 windows terminal 的設定中，點擊 *git bash* -> *外觀*，將文字改成下載的字體
3. 在 `C:\Program Files\Git\usr\bin` 內，擺入 `oh-my-posh.exe`
4. 在 git bash 上輸入 `$HOME`
5. 在顯示的資料夾位置裡面加入自己想要的主題檔，像是我想要的主題叫做 *paradox*，就可以從 [oh-my-posh themes](https://github.com/JanDeDobbeleer/oh-my-posh/tree/main/themes) 裡面找到名叫 [paradox.omp.json](https://github.com/JanDeDobbeleer/oh-my-posh/blob/main/themes/paradox.omp.json) 的 json 檔。
6.在 `$HOME` 顯示的資料夾位置內建立 `.bashrc` 檔。
7. 在 `.bashrc` 內輸入
```powershell
eval "$(oh-my-posh init bash --config ~/paradox.omp.json)"
```
> `paradox` 是主題的名稱，也可以換成其它自己想要的主題 

接下來重新開啟 terminal 新的主題就會開始套用了喔!