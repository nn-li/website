# HANDOFF — 震時秒判 案例頁調整

## 1. 正在改的檔案（完整路徑）

- `/Users/brynn/Github/website/work/quake.html` — 案例頁主檔
- `/Users/brynn/Github/website/assets/case.css` — 共用案例頁樣式
- `/tmp/userflow.html` — userflow 圖產生器（非網站檔，用 Playwright 渲染後截圖輸出）
- 圖片輸出目錄：`/Users/brynn/Github/website/assets/images/project/UIUX/web/quake/`

> 本機預覽：`python3 -m http.server 8000 --directory /Users/brynn/Github/website` → `http://localhost:8000/work/quake.html`
> 尚未 commit / push，全部只在本機。

---

## 2. 六項待辦狀態

| # | 項目 | 狀態 |
|---|------|------|
| 1 | 使用者流程斷層修復（搖晃結束後自動接續 → 避難路線導航） | ✅ 已完成 |
| 2 | 隱藏 Functional Map 區塊 | ✅ 已完成 |
| 3 | 主要負責項目加入「聲音設計、介紹影片製作」 | ✅ 已完成 |
| 4 | 副標題/內文斷行 → 改自然換行 | ✅ 已完成（走過彎路，見坑） |
| 5 | Persona 改孕婦、山區 | ⚠️ （Persona 換了，Journey Map 2待改版） |
| 6 | Prototype 三支影片對齊最下面 | ✅ 已完成 |
| 追加 | 刪除 `<p>` 的 `max-width: 64ch` | ✅ 已完成（未截圖驗證） |

---

## 3. 每項改動的具體位置

**#1 流程斷層** — `/tmp/userflow.html` 內 Flow 1（`#f1`）。
- 新增 `.elbow` class（含 `.ev1 / .eh / .ev2` L 型接線 + 箭頭）取代原本懸空的 `.mdown > .varw`。
- `.eh`（水平線）用百分比對齊：左端 13.3%（= 末排第一顆 node 中心）、右端 67.1%（= 合流 bar 中心），數值是用 Playwright 量出實際 node 中心校準的。
- 重新渲染輸出 → 覆蓋 `web/quake/userflow-1.png`。HTML 端引用維持 `userflow-1.png?v=2`。

**#2 Functional Map 隱藏** — `work/quake.html`，設計決策 section（`#s3`）內。
- 原「Functional Map」的 `<div class="sub">…</div>` 整塊用 `<!-- Functional Map（暫時隱藏） … -->` HTML 註解包起來（**沒刪，可還原**）。
- 已驗證頁面上 `img[src*=functional-map]` 不存在。

**#3 負責項目** — `work/quake.html`，`.cx-metarow` 內。
- `<dt>主要負責項目</dt><dd>` → `UX 研究、UI/UX 設計、Prototype、聲音設計、介紹影片製作`

**#4 自然換行** — `work/quake.html` 全頁 + `assets/case.css`。
- 移除 19 處 `<br class="pb" />`（replace_all）。
- `case.css` 移除 `@media (max-width:860px){ br.pb {display:none} }` 規則。

**#5 Persona** — `work/quake.html`，挑戰與洞察 section（`#s2`）「代表人物與旅程」`.sub`。
- 圖：`persona.jpg`（Henry 單人）→ 改成 `.fig.two` 並排 `persona-gill.jpg` + `persona-nora.jpg`。
- 其下另加一張 full-width `.fig` = `journey.jpg`（圖說「Journey Map · 從災害前到災後協助的行為與情緒歷程」，不再寫名字）。
- 文案改寫：Gill（25 歲孕婦，對安全需求更敏感）＋ Nora（21 歲山區居民，土石流風險、資訊中斷）。
- 新圖來源：`…/專案介紹素材/…persona & journey map圖檔/Persona_Gill.png`、`Persona_Nora.png`（皆 823×661），用 `sips -s format jpeg 88` 轉出到 `web/quake/persona-gill.jpg`、`persona-nora.jpg`。

**#6 Prototype 底部對齊** — `assets/case.css`。
- `.uclip` 加 `display:flex; flex-direction:column;`
- `.uclip video` 加 `flex:1 1 auto; min-height:0; object-fit:contain;`
- 原因：`proto-4-drill-select` 與 `proto-6-marquee` 是 600×1052，其餘是 600×1080，卡片高度不齊 → 讓影片撐滿、cap 貼底。

**追加：刪 64ch** — `assets/case.css`。
- `.cx-sec > .intro` 移除 `max-width: 64ch;`
- `.sub p` 移除 `max-width: 64ch;`
- （`.uidark .lede` / `.uphase p` / `.ffind p` 是 60ch，**未動**。）

---

## 4. 踩過的坑

- **#4 一開始做反了**：把「斷行」理解成「加手動 `<br>`」，插了 19 個 `<br class="pb">` + 寫了 mobile 隱藏規則。使用者澄清是要**自然換行、不要斷行**，全部退回。
  - 附帶認知修正：中文本來就會在任意字元換行（沒有詞邊界），所謂「詞中斷行」是正常 CJK 行為，不該用手動斷行去「修」。
- **打字手殘**：Functional Map 註解收尾處誤打了 `óó`，已修掉。
- （既往）Playwright 截底部圖時 lazy-load 圖是空的 → 需先 `img.loading='eager'` + 慢速捲動再截。
- （既往）port 8000 曾被舊 server 佔用指到錯目錄 → 要用 `--directory` 指定。
- `rm -rf` 被沙箱擋，清檔要靠使用者手動。

---

## 5. 尚未驗證 / 待確認

- **#5 Journey Map 不一致**：`journey.jpg` 仍是 **Henry** 的名字＋照片，和新的 Gill/Nora persona 對不上。目前用「不寫名字的圖說」淡化，但嚴格看仍不一致 → 需使用者**重出 Gill 或 Nora 版的 Journey Map** 才能真正對齊。
- **追加 64ch 刪除後未截圖**：刪完沒有重新渲染確認視覺，段落現在會撐滿欄寬，寬螢幕行長是否過長未看過。
- **手機版（<860px）**：persona 雙圖、64ch 移除後的窄螢幕呈現未特別截圖驗證。
- **Git**：以上全部未 commit / push。
