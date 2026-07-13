# 作品集網站 — 如何加入真實檔案

這是一個純靜態網站（放在 GitHub Pages），沒有後台。
「加內容」＝ **把檔案放進資料夾，再改 HTML 裡的路徑**。改完 push 上 GitHub 就會自動更新。

## 資料夾結構

```
website/
├─ index.html              ← 首頁（Hero / Work / About / Approach / Contact）
├─ README.md               ← 你正在看的這份
├─ assets/
│  ├─ case.css             ← 案例詳頁的共用樣式（首頁樣式在 index.html 裡）
│  ├─ images/              ← 所有圖片放這裡
│  │   ├─ ledger-cover.svg     （佔位圖，換成你的真圖）
│  │   ├─ atlas-cover.svg
│  │   ├─ northwind-cover.svg
│  │   └─ foglight-cover.svg
│  └─ files/
│      └─ (把 nina-cv.pdf 履歷放這裡)
└─ work/                   ← 每個作品一個詳頁
    ├─ _template.html      ← 新作品就複製這個
    ├─ ledger.html         ← 已填好的範例，照著改
    ├─ atlas.html
    ├─ northwind.html
    └─ foglight.html
```

## 三種「加真實檔案」的做法

### 1) 換作品封面圖
把你的圖（JPG / PNG / SVG 都可）放進 `assets/images/`。
- 最簡單：檔名取成跟佔位圖一樣（例如 `ledger-cover.svg`）直接覆蓋，不用改任何程式。
- 若用 JPG：放成 `ledger-cover.jpg`，然後把引用它的地方 `.svg` 改成 `.jpg`：
  - 首頁 `index.html` 的作品縮圖 `<img src="assets/images/ledger-cover.svg">`
  - 詳頁 `work/ledger.html` 裡的 `<img src="../assets/images/ledger-cover.svg">`
    （詳頁在子資料夾，所以路徑前面多一個 `../`）
- 建議尺寸：封面約 1200×750（16:10），檔案壓到 300KB 以內比較快。

### 2) 加履歷 PDF 下載
把履歷放成 `assets/files/nina-cv.pdf`。
首頁 Contact 區已經有下載按鈕（`Résumé ↓`）指向這個路徑。換名字的話同步改 `index.html` 裡那一行。

### 3) 寫每個作品的案例詳頁
打開 `work/ledger.html`，把裡面所有 `【___】` 和標了 `TODO` 的地方換成你的內容。
要新增一個作品：
1. 複製 `work/_template.html` → 改名（例：`work/my-project.html`）
2. 填內容、放封面圖
3. 回 `index.html` 的 Work 區塊，把某個 `<a class="project" href="work/xxx.html">` 指到你的新頁

## 把假資料換成你自己的（首頁 index.html）

搜尋並取代這些地方：
- `Nina Li` → 你的名字（出現在 `<title>`、nav 品牌 `Nina.Li`、Hero、footer）
- Hero 標題與那段自我介紹（約 345–356 行）
- About 段落與 `facts`（經歷、工具、語言，約 426–441 行）
- Contact 的 email `hello@ninali.design`
- 社群連結 LinkedIn / Dribbble / GitHub 的 `href="#"` 換成你的網址
- footer 的 `Placeholder content` 字樣

## 本機預覽

在 website 資料夾開一個小伺服器（圖片才載得出來）：

```bash
python3 -m http.server 8000
# 瀏覽器開 http://localhost:8000
```

## 發佈

```bash
git add -A
git commit -m "Add real portfolio content"
git push
```

GitHub Pages 會自動更新（通常 1 分鐘內）。
