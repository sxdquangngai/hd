---
title: 在 macOS Catlina 安裝 Vue.js
date: 2018-10-19 22:49:00
description: 學習 Vue.js ，首先得將「開發環境建置」的事情搞定。使用 macOS Catlina (V10.15) 作業系統的使用者，如何安裝及設定 Vue.js 相關的 SDK ，本文將有指引說明 How to ......
sidebar: auto
tags:
    - macOS Catlina
    - Vue.js
---

## 安裝作業

::: tip
【參考文件】： [Vue CLI](https://cli.vuejs.org/guide/prototyping.html)
:::

### 安裝前準備

Vue CLI 需搭配 Node.js 版本 8.11.0+ 。故請先用以下指令檢查目前使用中之 Node.js 版本。


    $ node -v


### 執行安裝指令


    $ npm install -g @vue/cli

或


    $ yarn global add @vue/cli

### 驗證安裝成功


    $ vue --version
    @vue/cli 4.0.4


## 值得安裝的 VS Code Extensions

採用 VS Code 作為程式編輯器者，可安裝下列之 Extensions，使 Vue.js App 開發作業更加順暢。 

- Vetur
- Vue VSCode Snippets
- Vue VS Code Extension Pack
- Vue 2 Snippets

::: tip
【參考文件】： [BEST VISUAL STUDIO CODE VUE.JS EXTENSIONS FOR SITECORE JSS DEVELOPMENT](https://www.kayee.nl/2019/03/11/best-visual-studio-code-vue-js-extensions-for-sitecore-jss-development/)
:::


## Node.js 版本控管工具：n


### 使用 n 進行版本控管

::: tip
【參考文件】： [n – Interactively Manage Your Node.js Versions](https://github.com/tj/n)
:::

 1. 安裝 n ：不使用 brew ，改用 n-install 方式安裝：

        $ curl -L https://git.io/n-install | bash


        $ which n
        /Users/<UserName>/n/bin/n
        
    【註】： 使用 brew install n 安裝，其安裝路徑如下

        $ which n
        /usr/local/bin/n
        
        $ ls -al /usr/local/bin/n
        lrwxr-xr-x  1 <UserName>  admin  23 10 19 10:37 /usr/local/bin/n -> ../Cellar/n/6.0.1/bin/n

 2. 設定環境變數 PATH ，以便 n 能順利執行。

    n-install 於安裝結束時，自動在 Shell 設定檔：`~/.bash_profile` (Bash Shell)，或 `~/.zshrc` (ZSH Shell) ，加入如下之 PATH 相關設定，以便 **`$HOME/n/bin`** 路徑也能加入環境變數 PATH 之中。

        export N_PREFIX="$HOME/n"; [[ :$PATH: == *":$N_PREFIX/bin:"* ]] || PATH+=":$N_PREFIX/bin"

 3. 重新啟動 Shell 。

        $ source ~/.bash_profile

    或

        $ source ~/.zshrc


### 解除安裝作業

使用 n-install 方式安裝， n 會安裝在以下目錄路徑：

    $HOME/n/bin

欲解除安裝，可透過 n-install 其自行提供的工具 n-uninstall 完成。

    $ n-uninstall

記得 `~/.bash_profile`, `~/.zshrc` 檔案，對於「環境變數」與 PATH 相關的設定亦需取消。

以 Bash Shell 為例：

1. 啟動文字編輯器。

        $ vim ~/.bash_profile 

2. 刪除以下這段設定。

        export N_PREFIX="$HOME/n"; [[ :$PATH: == *":$N_PREFIX/bin:"* ]] || PATH+=":$N_PREFIX/bin"

3. 取消已設定之環境變數。

        $ unset N_PREFIX

4. 重載 Bash Shell 設定。

        $ source ~/.bash_profile


### n 版本控管常用操作

**安裝 LTS 版本**

    $ n lts
    
      installing : node-v10.16.3
           mkdir : /Users/alanjui/n/n/versions/node/10.16.3
           fetch : https://nodejs.org/dist/v10.16.3/node-v10.16.3-darwin-x64.tar.gz
       installed : v10.16.3 to /Users/alanjui/n/bin/node
          active : v12.12.0 at /usr/local/bin/node

**安裝最近發行版本**

    $ n latest
    
      installing : node-v12.12.0
           mkdir : /Users/alanjui/n/n/versions/node/12.12.0
           fetch : https://nodejs.org/dist/v12.12.0/node-v12.12.0-darwin-x64.tar.gz
       installed : v12.12.0 to /Users/alanjui/n/bin/node
          active : v12.12.0 at /usr/local/bin/node


**查詢已安裝各版本**

    $ n ls
    node/10.16.3
    node/12.12.0


**切換版本**

    $ n
    
        node/10.16.3
        node/12.12.0
    
    Use up/down arrow keys to select a version, return key to install, q to quit


然後使用上、下鍵，標示欲擇用版本，按《Enter》鍵。

