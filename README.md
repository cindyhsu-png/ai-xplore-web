# AI-Xplore

AI 行銷模組銷售頁。純靜態單檔（`index.html`），走 GitHub Pages，作法比照 `nova-web` / `nschool-web` / `xlab-web`，之後可整頁 iframe 嵌入 Kolable。

## Session 結構

| Session | 內容 |
|---|---|
| 1 · 首頁 | 主標 **AI-Xplore**、副標，背景是 17 個模組排成三軌反向跑馬 + 99agent 主視覺緩慢交替 |
| 2 · 痛點鉤子（3 屏） | 2-A 三個陷阱／2-B 四步內容產線／2-C 不用每次從零開始（模組槽位） |
| 3 · 工具模組 | 全部模組卡片 + 分類篩選（內容策略／圖像／影片／音訊） |
| — | 方案定價、LP 嵌入區、結尾 CTA、footer |

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
