# Git 基礎：從零開始的手把手教學

> **目標**：搞懂 git 是什麼，會用基本指令記錄程式碼的每一個版本，改壞了也能回去。
> **時間**：順利的話大概 15 分鐘
> **範圍**：git init、git status、git diff、git add、git commit、git log，還有 Claude Code 的 `/commit`。推到 GitHub、branch、merge、衝突解決不在這次範圍內。

---

## Git 到底是什麼？

你程式碼的存檔系統。RPG 打 Boss 之前會先存檔吧？Git 就是這樣——覺得「這個版本還不錯」就存一個點，之後任何時候都可以回去。

全部都在你自己的電腦上發生，不需要網路，不需要帳號。

---

## 你需要準備的東西

- **Git**（版本控制工具本人）：確認有沒有 → 打開 Terminal，輸入 `git --version`。看到 `git version 2.x.x` 就有了。沒有 → 輸入 `xcode-select --install` 跟著步驟走。
- **Terminal**（打指令的視窗）：按 Cmd + Space，搜尋「Terminal」。
- **一個專案資料夾**：裡面有你正在寫的程式碼。還沒有的話，隨便建一個資料夾放幾個檔案也行。

---

## 先搞定 Terminal 基礎，60 秒

你不需要很懂 Terminal，知道怎麼去正確的地方就夠了。

### 第 1 步：打開 Terminal
**做這個**：開 Terminal。
**在哪裡**：按 Cmd + Space，打 "Terminal"，按 Enter。
**你會看到**：一個視窗，裡面有一行文字，最後有閃爍的游標。
**確認**：視窗打開，游標在閃。
**沒出現的話**：去 Applications → Utilities → Terminal。

### 第 2 步：去你的專案資料夾
**做這個**：輸入 `cd` 加上你的專案路徑。
**在哪裡**：Terminal 視窗裡。
**輸入這個**：`cd /Users/你的名字/Projects/你的專案名稱`
**你會看到**：游標跑到下一行，沒有任何錯誤。
**確認**：輸入 `pwd` 會顯示你現在在哪裡，應該是你的專案路徑。
**沒出現的話**：在 Finder 找到你的專案資料夾，在 Terminal 打 `cd `（空格），然後把資料夾拖進 Terminal 視窗裡，按 Enter。

---

## 開始

### 第 3 步：建立 git 倉庫
**做這個**：輸入 `git init`。
**在哪裡**：Terminal 視窗裡，確認你在專案資料夾內。
**輸入這個**：`git init`
**你會看到**：
```
Initialized empty Git repository in /Users/你的名字/Projects/你的專案名稱/.git/
```
**確認**：有看到 `Initialized` 這個字。
**沒出現的話**：確認你在正確的資料夾裡（回去第 2 步）。

這一步只需要做一次。它會在你的資料夾裡建一個隱藏的 `.git` 資料夾，git 所有的紀錄都存在裡面。之後不用再跑第二次。

---

如果你的專案已經有 `.git` 資料夾（例如從別人那邊拿來的），跳過這步。怎麼確認？輸入 `git status`，沒有噴 `fatal: not a git repository` 就是已經有了。

---

### 第 4 步：看看現在的狀態
**做這個**：輸入 `git status`。
**在哪裡**：Terminal 視窗裡。
**輸入這個**：`git status`
**你會看到**：
```
On branch main
Untracked files:
        src/pages/booking.tsx
        src/utils/helpers.ts
```
或是 `nothing to commit, working tree clean`（表示你最近沒改東西）。
**確認**：沒有看到 `fatal:` 開頭的錯誤。
**沒出現的話**：如果看到 `fatal: not a git repository`，回去第 3 步跑 `git init`。

---

**看懂那些字在說什麼：**
- **Untracked files** → 新檔案，git 還沒開始追蹤它
- **modified: xxx** → 這個檔案你改過了，但還沒存到 git 歷史紀錄
- **nothing to commit** → 沒有任何改動

---

### 第 5 步：看看你到底改了什麼
**做這個**：輸入 `git diff`。
**在哪裡**：Terminal 視窗裡。
**輸入這個**：`git diff`
**你會看到**：
```
- const name = "舊的內容";
+ const name = "新的內容";
```
前面有 `-` 的是刪掉的行（紅色），`+` 的是新加的行（綠色）。
**確認**：有看到你改過的內容。如果什麼都沒顯示，表示沒有已追蹤檔案被修改（新檔案不會出現在 `git diff`）。
**沒出現的話**：畫面被佔滿了的話，按 `q` 離開。

這步不是必要的，但養成 commit 之前先看一眼 diff 的習慣很好。確認你存的是你真的想存的東西。

### 第 6 步：把改動放進暫存區
**做這個**：輸入 `git add .`（注意最後有個點）。
**在哪裡**：Terminal 視窗裡。
**輸入這個**：`git add .`
**你會看到**：沒有任何輸出——這是正常的，代表成功。
**確認**：再輸入一次 `git status`，剛才的檔案會出現在 `Changes to be committed:` 底下。
**沒出現的話**：確認有打那個點（`.`），點代表「全部改過的檔案」。

暫存區（Staging Area）是什麼？想像你要寄包裹：先把東西擺到桌上（`git add`），確認都放好了，才打包寄出（`git commit`）。那個桌子就是暫存區。

### 第 7 步：存一個紀錄點
**做這個**：輸入 `git commit -m` 加上你的訊息。
**在哪裡**：Terminal 視窗裡。
**輸入這個**：`git commit -m "新增預約頁面的表單驗證"`（引號裡換成你這次改了什麼）
**你會看到**：
```
[main 3a9f2c1] 新增預約頁面的表單驗證
 1 file changed, 24 insertions(+), 3 deletions(-)
```
**確認**：有看到 `[main xxxxxxx]` 開頭的那行。
**沒出現的話**：確認 `-m` 後面有引號，訊息在引號裡面。

### 第 8 步：看看歷史紀錄
**做這個**：輸入 `git log --oneline`。
**在哪裡**：Terminal 視窗裡。
**輸入這個**：`git log --oneline`
**你會看到**：
```
3a9f2c1 新增預約頁面的表單驗證
abc1234 修正登入頁面的 bug
def5678 初始化專案
```
**確認**：你剛才存的那條紀錄出現在最上面。
**沒出現的話**：如果畫面被佔滿進入翻頁模式，按 `q` 離開。

### 第 9 步：用 Claude Code 幫你 commit
**做這個**：在 Claude Code 的對話框裡輸入 `/commit`。
**在哪裡**：Claude Code 的輸入框。
**輸入這個**：`/commit`
**你會看到**：Claude 分析你改了什麼，自動寫好 commit 訊息，問你要不要確認。
**確認**：訊息看起來合理，按確認送出。
**沒出現的話**：確認你有先存好所有開啟的檔案（Cmd + S）。

這步等於幫你做了第 6 步 + 第 7 步，而且 commit 訊息它幫你想好。懶得自己寫訊息的時候用這招。

---

## 搞定了。你剛剛做了什麼？

Git 幫你記錄程式碼的每一個版本，全部存在你自己的電腦上。`git init` 是開啟這個功能，`git add` 是選要記哪些改動，`git commit` 是存到歷史紀錄。每個 commit 就是一個存檔點，改壞了可以回去。

---

## 可能會出包的地方

**`fatal: not a git repository`**
你不在一個有 git 的資料夾裡。先 `cd` 到你的專案資料夾，然後跑 `git init`。

**`nothing to commit, working tree clean`**
還沒改任何東西，或改了但還沒存檔。先按 Cmd + S 存一下。

**`error: src refspec main does not match any`**
還沒做過任何 commit。先跑 `git add .` → `git commit -m "第一次 commit"`。

**commit 的訊息要寫什麼？**
寫你「改了什麼」就好。「修正登入 bug」「新增預約頁面」「更新首頁文案」——讓未來的自己看到知道當時在幹嘛就行了。或是用 `/commit` 讓 Claude 幫你寫。

**按了 `git diff` 或 `git log` 之後畫面卡住？**
你進入了分頁模式。按 `q` 就能離開，回到正常的 Terminal。

