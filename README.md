# Decision Experiments 決策小實驗

Managerial Psychology · Week 11 · Individual Decision-Making Biases（個人決策偏誤）

這是課堂用的線上作答網頁。學生用學號和姓名登入後，系統會隨機分派 A 或 B 版本，學生依老師指示逐題作答，答案即時存進老師的 Google 試算表。

This is an in-class response page. Students sign in with their student ID and name, are randomly assigned to Form A or Form B, and answer each experiment when the teacher opens it. Answers are saved to the teacher's Google Sheet.

---

## 課堂上的四個實驗

| # | 實驗 | 心理現象 | A / B 版差異 |
|---|---|---|---|
| 1 | How Many Stores? 7-ELEVEN 門市數 | 錨定效應 Anchoring | 參考數字：A 版 1,000／B 版 20,000 |
| 2 | Who Is Linda? | 代表性捷思、合取謬誤 Representativeness, conjunction fallacy | 兩版相同 |
| 3 | Save the Jobs 保住工作 | 框架效應 Framing effect | A 版「保住 200 個工作」／B 版「400 人失業」 |
| 4 | Flip a Coin 擲硬幣 | 損失規避 Loss aversion | 兩版相同 |
| 5 | Reflection 我的決定 | 課程最後的反思 | 兩版相同 |

學生看不到自己是哪一版。

---

## 運作方式

```
學生手機（GitHub Pages：index.html）
        │  學號＋姓名、作答內容
        ▼
Google Apps Script 網頁應用程式（/exec）
        │  比對名單、分派版本、寫入答案
        ▼
Google 試算表（只有老師看得到）
  ├─ Roster     名單：A 欄學號、B 欄姓名
  ├─ Responses  每位學生一列答案
  ├─ Settings   B1 控制開放到第幾題
  └─ Summary    自動統計，揭曉時投影
```

- **名單比對**：學號與姓名都要和 Roster 相符才能登入。比對時會忽略空白和英文大小寫。
- **隨機分派**：A、B 兩版人數保持平衡；同一位學生重新登入，版本不變。
- **每題只能送出一次**：鼓勵學生憑直覺作答。
- **名單與答案不在這個 repository**：這裡只有網頁，資料都存在老師的 Google 試算表。

---

## 檔案

| 檔案 | 說明 |
|---|---|
| `index.html` | 學生作答網頁。`API_URL` 填 Apps Script 的 `/exec` 網址 |
| `README.md` | 本說明 |

後台程式 `Code.gs` 放在 Google 試算表的 Apps Script 裡，不放在這裡。

---

## 上課時的操作

打開 Google 試算表的 **Settings** 分頁，修改 **B1**：

| B1 | 學生看到 |
|---|---|
| 0 | 只能登入，題目全部鎖住 |
| 1 | 開放實驗一 |
| 2 | 開放到實驗二 |
| 3 | 開放到實驗三 |
| 4 | 開放到實驗四 |
| 5 | 開放 Reflection |

學生的網頁約每 8 秒檢查一次，會自動解鎖新題目。

揭曉結果時投影 **Summary** 分頁，內容包括：
- 實驗一：A、B 兩版估計值的中位數
- 實驗二：選 (a)、(b) 的比例
- 實驗三：A 版選 Plan A、B 版選 Plan C（穩定方案）的比例
- 實驗四：最低金額的中位數

---

## 建置步驟

### 1. Google 試算表與 Apps Script

1. 建立 Google 試算表，把名單分頁命名為 **Roster**，第 1 列填 `StudentID | Name`，從第 2 列開始放名單。學號欄先設為「純文字」，避免開頭的 0 不見。
2. 在試算表點 **擴充功能 → Apps Script**，貼上 `Code.gs`，然後儲存。
3. 函式選單選 **setup**，按 **執行**，並完成授權（進階 → 前往（不安全）→ 允許）。
4. 點 **部署 → 新增部署作業**，在 ⚙️ 選 **網頁應用程式**：
   - 執行身分：**我**
   - 具有存取權的使用者：**所有人**
5. 複製 `/exec` 網址。用瀏覽器打開時，看到 `{"ok":true,...}` 就表示成功。

### 2. GitHub Pages

1. 把 `index.html` 裡的 `API_URL` 改成 `/exec` 網址，上傳到這個 repository 的最外層。
2. 到 **Settings → Pages**，Source 選 **Deploy from a branch**，Branch 選 **main / (root)**，按 **Save**。
3. 1–3 分鐘後，網站會出現在 `https://<帳號>.github.io/<repository 名稱>/`。

### 3. 課前測試

1. 用名單上的一位學生登入。
2. 把 Settings!B1 改成 1，確認實驗一可以送出，Responses 也出現資料。
3. 測完刪除測試資料，B1 改回 0。

---

## 修改

- **修改網頁**：在 GitHub 編輯 `index.html` 後 Commit，約 1 分鐘生效。
- **修改後台程式**：在 Apps Script 點 **部署 → 管理部署作業 → 編輯 → 版本：新版本 → 部署**。這樣網址不變，`index.html` 就不用改。
- **換班級**：每個班級各用一份試算表，各自部署一個 `/exec` 網址，再各放一份網頁。

---

## 常見問題

| 狀況 | 處理方式 |
|---|---|
| 網站顯示 404 | 等幾分鐘；確認檔名是 `index.html`，而且放在最外層 |
| 登入時顯示「網路問題」 | `API_URL` 貼錯，或部署時沒有選「所有人」 |
| 顯示「學號或姓名與名單不符」 | 分頁名稱不是 Roster、學號開頭的 0 不見了，或姓名有異體字 |
| 題目一直沒解鎖 | 確認 Settings!B1 的數字；學生可以重新整理頁面 |

---

## 隱私

學生的學號、姓名和答案只存在老師的 Google 試算表，不會出現在這個公開的 repository。學期結束後，請刪除或封存該試算表。
