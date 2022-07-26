---
title: 執行完 pipenv 後 Shell Prompt 怪怪的
date: 2018-10-18 08:32:00
description: 使用 pipenv shell 啟動「Python虛擬環境」後，Shell 的提示符號之前，原本該顯示的「使用者帳號」、「目前所在目錄路徑」全都不見了⋯⋯
sidebar: auto
tags:
    - Python
    - VirtualEnv
    - pipenv
---

在我的 macOS 作業系統上，使用 ZSH 當成 Command Line 的操作介面時，天空是藍色的，以 pipenv 管理 Python 的虛擬環境時，一切運作正常。


![](https://paper-attachments.dropbox.com/s_E0FE9E5474053ADFE59BF139FA39C215CCC86B81A5277B5AE04311F8F42685B9_1571196203931_image.png)


但是這個看來賞心悅目的操作介面，在撰寫技術文件的時候，卻是個麻煩。因為在 Copy / Paste ，置入了文字編輯器後，呈現的系「亂碼二分之一」不忍卒睹的結果！  😿

      ~    11:16:51 
       cd ~/workspace/Python/django_002
    
      ~/workspace/Python/django_002     master      11:17:19 
       pipenv shell
    Launching subshell in virtual environment…
     . /Users/alanjui/.local/share/virtualenvs/django_002-Jo8xwSBw/bin/activate
    
      ~/workspace/Python/django_002     master      11:17:44 
        . /Users/alanjui/.local/share/virtualenvs/django_002-Jo8xwSBw/bin/activate
    
       django_002-Jo8xwSBw   ~/workspace/Python/django_002     master      11:17:45 
      

天才的我，想說：「不然這樣好了！寫程式、做開發的時候，用這外觀看起來美美的 ZSH Shell；但在撰寫技術文件的時候，咱就改用成樸實的 Bash Shell 吧！」

唉呀！我真是太聰明了，這款小代誌，怎可能難倒咱家呢！

待 Bash Shell 的設定完成，開始啟用之後⋯⋯

悲劇了！！  


## 問題狀況

 1. 進入 Django 專案的根目錄：

        alanjui@MBP-2018:~/workspace/Python/django_002 (master) 
        $ 

 2. 輸入指令：`pipenv shell` ，啟動 Django 專案所使用之 **Python 虛擬環境**。

        alanjui@MBP-2018:~/workspace/Python/django_002 (master) 
        $ pipenv shell
        Launching subshell in virtual environment…
        bash-5.0$  . /Users/alanjui/.local/share/virtualenvs/django_002-Jo8xwSBw/bin/activate
        (django_002) bash-5.0$ 

 3. Shell Prompt 顯示的格式，不再是原先的：**《使用者帳號》@《電腦名稱》**。

    ![](https://paper-attachments.dropbox.com/s_E0FE9E5474053ADFE59BF139FA39C215CCC86B81A5277B5AE04311F8F42685B9_1571194279863_image.png)


## 問題解析

masOS 作業系統的 Bash Shell ，其「預設設定檔」為： `~/.bash_profile`，不似 Linux 作業系統為： `~/.bashrc` 。

但 **pipenv shell** 啟動後，會對 Prompt 顯示的設定做變更，其依據為： `~/.bashrc` 檔內的設定 ，而不是 macOS 預設的 `~/.bash_profile` 。

所以，新增個 `~/.bashrc` 檔，使指令：`source ~/.bashrc` 的執行，如同直接執行：`source ~/.bash_profile`。

```shell{2:2}
if [ -f ~/.bash_profile ]; then
    source ~/.bash_profile
fi
```


## 排解方法

 1. 透過指令：exit，先關閉已啟動之 Python 虛擬環境。

        (django_002) bash-5.0$ exit
        exit
        
        alanjui@MBP-2018:~/workspace/Python/django_002 (master) 
        $ 

    ![](https://paper-attachments.dropbox.com/s_E0FE9E5474053ADFE59BF139FA39C215CCC86B81A5277B5AE04311F8F42685B9_1571198677813_image.png)


 2. 新增檔案： ~/.bashrc，檔內設定如下：

    啟動編輯器：

        $ vim ~/.bashrc
    
    加入下述內容：

    ```shell
    if [ -f ~/.bash_profile ]; then
        source ~/.bash_profile
    fi
    ```

 3. 回到 Django 專案目錄，透過指令：pipenv shell，重新啟動 Python 虛擬環境。

        alanjui@MBP-2018:~/workspace/Python/django_002 (master) 
        $ pipenv shell

    ![](https://paper-attachments.dropbox.com/s_E0FE9E5474053ADFE59BF139FA39C215CCC86B81A5277B5AE04311F8F42685B9_1571195435195_image.png)



## 參考文件


- [Git PS1 error in pipenv shell #844](https://github.com/pypa/pipenv/issues/844)
- [.bash_profile 與 .bashrc 的差異](http://jamestw.logdown.com/posts/283485--bash-profile-bashrc-difference?source=post_page-----4834eaf73379----------------------)



