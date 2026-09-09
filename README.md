# AI-Xplore

AI 行銷模組銷售頁。純靜態單檔（`index.html`），走 GitHub Pages，作法比照 `nova-web` / `nschool-web` / `xlab-web`，之後可整頁 iframe 嵌入 Kolable。

## Session 結構

| Session | 內容 |
|---|---|
| 1 · 首頁 | 主標 **AI-Xplore**，背景是 99agent 真實作品組成的 **3D 電視牆**（9 軌反向直向捲動、透視傾斜），點任一格放大播放 |
| — | 數字見證：87+ / 11.8× / 4,800萬+ / 78%，進入視窗時往上跳動計數 |
| 2-A · 痛點鉤子 | 三個陷阱：數位垃圾／幻想一鍵變現／只靠單點工具 |
| 2-B · 產線 × 數據 | 四步產線每段掛量化指標，右側是成效儀表板（KPI／ROAS 趨勢 SVG／活躍活動表） |
| 2-C · 真實案例 | 26 支 AI UGC 影片 + 50 張 AI 生成廣告圖，可切換、可展開全部、點擊放大 |
| 3 · 客戶見證 | 6 則企業主見證橫向跑馬，hover 暫停 |
| 4 · 模組地圖 | 全部模組卡片 + 分類篩選 |
| 5 · 免費贈課 | AI 自動化工作流實戰班（原價 NT$5,800 → NT$0），8 堂課大綱 |
| — | 方案定價、LP 嵌入區、結尾 CTA、footer |

全站游標為光點暈染特效（觸控裝置自動停用）。

## 素材來源

`assets/portfolio.js` 是從 <https://stock.99agent.app/> 的公開素材庫抓下來的清單（26 支影片 + 50 張圖），檔案本身放在 99agent 的公開 GCS：

```
https://storage.googleapis.com/99agent-public/portfolio/{images,videos}/<檔名>
```

網站直接引用該網址，不另存本地檔。要換素材就改 `assets/portfolio.js` 裡的 `STOCK_VIDEOS` / `STOCK_IMAGES`。

## 改內容

**模組清單**：改 `index.html` 裡的 `const MODULES = [...]`（約第 690 行）。
每筆欄位：

```js
{n:"模組名稱", cat:"image", ic:"i-image", status:"live", price:"3 點/張", d:"說明文字"}
```

- `cat`：`content` / `image` / `video` / `audio`
- `status`：`live` 現貨即開通、`soon` 半年內交付
- `ic`：`i-news` `i-script` `i-image` `i-video` `i-audio` `i-person`

`RESERVED` 會自動算出「24 − 已列出模組數」，顯示成「還有 N 個模組在路上」的卡片。

## 嵌入 LP

把收單頁網址填進 `#lpEmbed` 的 `data-src`：

```html
<div class="embed" id="lpEmbed" data-src="https://your-lp-url">
```

有值才會長出 iframe，沒填就顯示佔位說明。

## 購物車按鈕落點

頁面上預留了 `.cta-slot`，外部購物車元件掛進去後預設按鈕會自動隱藏：

| `data-cta` | 位置 |
|---|---|
| `nav` | 導覽列右側 |
| `hero-primary` | 首頁主 CTA |
| `plan-starter` | 行銷工具人方案 |
| `plan-master` | 行銷大師方案 |
| `final` | 結尾 CTA |
| `module` | 每張模組卡片（`data-sku` 為模組名稱） |

掛法：

```js
AIXplore.mountCTA('hero-primary', myCartButtonElement);
AIXplore.slots();   // 列出所有落點與 sku
```

或直接在 HTML 裡把 `.cta-slot` 內的 `.cta-fallback` 換成自己的按鈕。

## 部署

推 `main` 就自動由 `.github/workflows/deploy.yml` 發佈到 GitHub Pages。
