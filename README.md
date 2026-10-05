# 平行閃卡共享圖鑑（PTCGP 豪華擴充包：Mega B4b）

朋友之間共用的平行閃卡登記表：每個人登記自己有幾張、看出誰還沒解鎖、哪些卡全隊都沒有，並依遊戲交換規則（同稀有度一換一）自動建議怎麼換最有效率。

## 檔案說明

- `index.html`：整個網站，只有這一個檔案。
- `firestore.rules`：Firebase 資料庫的安全規則。

## 先試用（不用任何設定）

直接用瀏覽器打開 `index.html` 就是「示範模式」，會有三位假玩家和隨機資料，可以先試按看看。示範模式的資料只存在你這台裝置。

## 正式上線：三個步驟

### 一、建立 Firebase 專案

1. 到 https://console.firebase.google.com 用 Google 帳號登入，按「新增專案」，名稱隨意（例如 `b4b-cards`），Google Analytics 可以關掉。
2. 左側選單「建構 → Authentication」→「開始使用」→ 在「登入方式」分頁啟用 **匿名**。
3. 左側選單「建構 → Firestore Database」→「建立資料庫」→ 位置選 `asia-east1`（台灣）→ 選「正式版模式」。
4. 在 Firestore 的「規則」分頁，把 `firestore.rules` 的內容整段貼上取代原本的，按「發布」。
5. 回到專案首頁，按「</>」（網頁）圖示新增網頁應用程式，名稱隨意，不用勾 Hosting。完成後會看到一段 `firebaseConfig = { ... }`，把大括號整段複製下來。

### 二、把設定貼進 index.html

用任何文字編輯器打開 `index.html`，找到最上方設定區的這一行：

```js
const FIREBASE_CONFIG = null;
```

把 `null` 換成剛剛複製的設定，例如：

```js
const FIREBASE_CONFIG = {
  apiKey: "AIza...",
  authDomain: "b4b-cards.firebaseapp.com",
  projectId: "b4b-cards",
  storageBucket: "b4b-cards.appspot.com",
  messagingSenderId: "123456789",
  appId: "1:123456789:web:abc123"
};
```

這組設定放在公開網頁上是正常的，它不是密碼；真正保護資料的是第一步發布的安全規則。

### 三、放上 GitHub Pages

1. 在 GitHub 建一個新的 repository（公開），上傳 `index.html`。
2. 到 repository 的「Settings → Pages」，Source 選「Deploy from a branch」，Branch 選 `main`、資料夾 `/ (root)`，按 Save。
3. 等一兩分鐘，網址會是 `https://你的帳號.github.io/repository名稱/`。
4. 回到 Firebase：「Authentication → 設定 → 授權網域」，新增 `你的帳號.github.io`。沒加的話匿名登入會失敗。

把網址傳給朋友，每個人第一次打開時新增自己的暱稱就能開始登記。

## 使用方式

- **登記**：在「我的登記」按卡片下方的 + 或 −。第一次 +1 的卡會有閃卡光效，表示新解鎖。
- **已送走的卡**：點卡圖打開詳情，勾選「已解鎖過」，張數為 0 也會算在圖鑑裡。
- **快速找卡**：上方可以搜尋中英文名稱或編號，並用狀態、屬性、稀有度篩選，條件可以疊加。
- **全隊進度**：看每個人的完成度、全隊都沒有的卡，以及哪些卡還有人缺、誰有多的可以送。
- **交換建議**：系統依「同稀有度一換一」配對，優先排「雙方都能解鎖新卡」的交換。先在遊戲中換好，再按「記錄這筆交換」，雙方張數會自動更新。
- **交換紀錄**：記錄錯了可以按「復原」。

## 注意事項

- 這個版本採「大家都能改」的設計：任何拿到網址的人都能修改任何玩家的資料，但安全規則禁止刪除玩家和竄改交換紀錄。網址請只分享給朋友。
- 要刪除玩家，請到 Firebase 主控台的 Firestore 手動刪除 `players` 裡對應的文件。
- 卡圖從外部網站載入，載入失敗時會改顯示屬性色卡，不影響登記。圖片版權屬於寶可夢公司，這個工具適合朋友間私下使用。
- 訓練家卡（物品、道具、支援者、競技場）的中文名稱是暫譯，可能和遊戲內不同。要修正的話，在 `index.html` 裡搜尋英文名稱，修改旁邊的中文即可。
- 免費方案每天有 5 萬次讀取、2 萬次寫入的額度，十幾二十人使用綽綽有餘。
