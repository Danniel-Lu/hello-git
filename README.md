# git 學習心得

## 1. 什麼是 git

git 是「分散式」版本控制

那什麼是版本?

- 每次更新的檔案
- 每一次的更改
- 每次更新的紀錄

---

## 2. 安裝 git

1. [先到 git 網站](https://git-scm.com/)
2. 右下點選 download for Windows 就會進到另一個網址並自動下載
3. 安裝 git.exe(好像一直下一步就好)<br>提示: 安裝路徑中不要有中文字，容易出錯。<br>[(可以參考這部五倍紅寶石的影片)](https://www.youtube.com/watch?v=8ke1jHHysBE&t=7s)
4. 左下角的開始找尋 Git Bash

確認電腦裡是否有安裝過 git:

```bash=
git --version
```

---

## 3. 設定環境變數

目的:告訴 git 這台電腦的使用者是誰，牽涉到後面使用 github。

```bash=
# 設定環境變數
$ git config --global user.name "DanLu"
$ git config --global user.email "dandesign0818@gmail.com"

# 確認設定的內容
$ git config --list

# 其實設定檔是存在 ~/.gitconfig 這個檔案 也可以在裡面修改
cat ~/.gitconfig
```

---

## 4. 建立 repo

repository (儲存庫) --> repo

不可能整個電腦都給 git 管，這樣範圍會太大。
所以每一個專案的進度(版本)都是獨立控管

在要被控管的專案(ex: hello-git 資料夾)底下，輸入這個指令

```bash=
# init 是 initial的縮寫
$ git init
```

會在這個 hello-git 內建立了 .git 檔案夾，之後所有關於 git 管理的資訊都會存在這個檔案夾。

Q: 如果 hello-git 這個檔案夾(專案) 不想再被 git 管理了？<br>
A: 直接把整個 .git 檔案夾刪掉 `rm -r .git`

![](https://i.imgur.com/iYbhwu4.png)

暫存區域: 放準備要提交的檔案 (這一次要 commit 的)

---

## 5. 在 repo 中新增、修改、刪除檔案

### 檔案狀態

下面示意圖說明了透過指令改變狀態的生命週期，事實上，這些改變的過程，都是在更新「索引檔」的過程。

![](http://git-scm.com/figures/18333fig0201-tn.png)

- **untracked** (未追蹤的，代表尚未被加入 Git 儲存庫的檔案狀態)
- **unmodified** (未修改的，代表檔案第一次被加入，或是檔案內容與 HEAD 內容一致的狀態)
- **modified** (已修改的，代表檔案已經被編輯過，或是檔案內容與 HEAD 內容不一致的狀態)
- **staged** (等待被 commit 的，代表下次執行 git commit 會將這些檔案全部送入版本庫)
  [圖片參考網址](https://ithelp.ithome.com.tw/articles/10134531)

查看 git 的狀態

```bash=
$ git status
```

把檔案加入

```bash=
$ git add <file>
```

提交這次的修改

```bash=
$ git commit -m "正確填寫這次 commit 的資訊"
```

檢視這次修改的差異
工作目錄與暫存區域的比較

```bash=
$ git diff <file>
```

暫存區域與儲存庫的比較

```bash=
$ git diff --cached <file>
```

已經加入過的檔案，又有修改的話，一樣要再 add 一次，才可以 commit

```bash=
$ git add <file>
$ git commit -m "xxxx"
```

針對已經加入過的檔案，可以直接用以下指令提交
這邊的 -a 代表 add

```bash=
$ git commit -am "xxxxx"
```

可以「反悔」加入暫存區

```bash=
$ git restore --staged <file>
```

反悔剛剛的修改
以前的指令是 checkout
連刪除檔案也可以反悔

```bash=
$ git restore <file>
```

將刪除狀態加入暫存區
rm 跟 add 都可以，因為 git 是更新狀態

```bash=
$ git rm/add <file>
```

讓檔案回到某個特定版本
\<commit> 通常是複製 6~7 碼，重複的機率很低

```bash=
$ git checkout <commit> test.txt
```

讓檔案回復到前兩個版本

```bash=
$ git checkout HEAD~2 test.txt
```

回到現在版本

```bash=
$ git checkout HEAD
```

- Untracked files: 代表還沒加入給 git 管的檔案(不是每個檔案都要給 git 管)
- git 的單位不是檔案，而是每次的修改。

#### `git log` 相關指令

```bash=
# 查看提交紀錄
$ git log

# 查看特定檔案的紀錄
$ git log <file>

# 查看檔案修改細節
$ git log -p <file>

# 可以搜尋關鍵字
$ git log --grep="delete"

# 查看內容是誰編寫的
$ git blame test.txt
```

### git 運作流程

圖中的 git checkout (舊寫法) 應該為 git switch (新寫法)
![](https://ithelp.ithome.com.tw/upload/images/20191013/20118842jcxM2UUNXC.png)
[圖片參考網址](https://ithelp.ithome.com.tw/articles/10227655?sc=rss.iron)

---

## 6. 建立分支(branch)與合併(merge)

[分支模擬工具](https://git-school.github.io/visualizing-git/)

預設分支: master / main

> Github 預設為 main，所以用 main 比較好<br>

### **把 git 預設分支改成 main**

進入 .gitconfig

```bash=
$ nano ~/.gitconfig
```

加入下面這段

```
[init]
        defaultBranch = main
```

如果 hello-git 已經是 master 的話可以執行以下指令把主分支改成 main

```bash=
$ git branch -m master main
```

### **branch**

```bash=
# 檢視分支
$ git branch

# * 是標注當前所在的分支

# 建立分支
$ git branch <branch-name>

# 切換分支
$ git switch <branch-name>
```

### **merge**

情境:
分支 main: 主分支
分支 login: 這是用開發 login 功能的

當我們把 login 功能完成後，必須把這個 login 分支「merge」回主分支去

```bash=
# 先切回main分支
$ git switch main

# merge main 跟 login
$ git merge login
```

如果兩個分支有**提交過同一個檔案的修改**，那就有可能發生衝突 conflict，此時就需要**解衝突**。
_ex: main 有增加 h1，login 有增加 input_
只能人工修改，需要跟同事討論
解完衝突之後，就要再 commit 一次 (bash 有提示)

當發 pull request 的時候，有衝突，要求開發的人回到自己的電腦，
把 main merge 進去 login 一次，再 push 一次這個 login。

## 0. 常用的終端機指令

| Windows | MacOS/Linux | 說明                                     |
| ------- | ----------- | ---------------------------------------- |
| cd      | cd          | 切換目錄 change directory                |
| cd      | pwd         | 顯示目前所在路徑 print working directory |
| dir     | ls          | 列出目前檔案夾的檔案列表 list            |
| mkdir   | mkdir       | 建立新的檔案夾 make dir                  |
| copy    | cp          | 複製檔案 copy                            |
| move    | mv          | 移動檔案 move                            |
| del     | rm          | 刪除檔案 remove                          |
| cls     | clear       | 清除畫面上的內容                         |
| cat     | cat         | 印出檔案內容                             |

> **windows 的 Git Bash 可以用 Mac 的指令**

```bash=
# 建立 hello-git 檔案夾
mkdir hello-git

# 進入 hello-git 檔案夾
cd hello-git

# 建立檔案 test.txt
touch test.txt

# 在Git Bash修改檔案
nano <fileName>

1. 修改完按 ctrl+x
2. Save modified buffer?(有沒有要存檔)   按 y
3. File Name to Write [DOS Format]: <fileName>(有沒有要改檔名)  按 enter

# 檢視檔案內容
cat test.txt

# 列出檔案
ls

# 列出全部檔案（列出隱藏檔）
ls -a

# 以完整格式列出檔案
ls -l

# 以完整格式列出所有的檔案
ls -al
```

**參數可以使用多個**<br>
參數的意義，不同指令會不一樣，但通常會是這樣<br>
-a => all 全部--------ex: ls -a 列出全部檔案（列出隱藏檔<br>
-r => recursive ->可以刪除每一層資料夾<br>
-f => force 強制 \* => 全部<br>
/ => 根目錄<br>
ex: rm -rf /\* =>電腦會變磚塊，不能亂下<br>

```bash
. 目前的檔案夾位置
.. 上一層檔案夾
~ 我的目錄，就是目前這個「使用者的目錄」，像是 /Users/aaa 之類的

# 回到使用者目錄或是 home 目錄
cd ~
# 回到上一層
cd ..
# 回到上兩層
cd ../..
```
