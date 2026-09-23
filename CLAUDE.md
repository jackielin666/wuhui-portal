# CLAUDE.md — 五惠系統入口網站 開發規範

## 專案概述
- 五惠食品內部「系統入口網站」，集中連結公司各應用系統。
- 使用者：管理部 Jackie（維護者，程式新手）與主管。說明請用繁體中文、一步一步講清楚。
- 純靜態網站：`index.html` + `systems.json` + `logo.png`。不需要後端，也不需要建置步驟（no build）。

## 最重要的原則：只改 systems.json
- 新增、修改、隱藏系統 → **只改 `systems.json`**，不要動 `index.html`。
- `index.html` 只在「版面／功能」要改時才修改。
- 系統清單的**正本**在 Google Sheet「五惠系統清單」。`systems.json` 是它的網站版副本，兩邊要同步。
- 修改 `systems.json` 時，順手把最上方的 `"updated"` 改成當天日期（YYYY-MM-DD）。

## 安全規範
- 不做登入功能，任何檔案都不存放帳號、密碼、Token 或 API Key。
- `index.html` 必須保留 `<meta name="robots" content="noindex, nofollow">`。
- 不引用外部 CDN、字型或追蹤碼。所有資源都放在 Repo 內。

## 配色規範
| 用途 | 色碼 |
|---|---|
| 頁面底色 | `#F6F6F4` |
| 卡片 | `#FFFFFF` |
| 文字與點綴 | `#2C2C2A` |
| 次要文字 | `#6B6B66` |
| 分隔線／卡片外框 | `#E3E3DE` |
| 系統圖示預設底色 | `#3F4A56` |

- **五惠紅 `#D7000F` 只能出現在 Logo**，介面其他地方一律不用紅色（包含 focus 框、hover、標籤）。
- 有代表色的系統，圖示背景使用系統色：
  - 廠務設備日常巡檢 `#0F5C8C`
  - 智慧環境5S巡檢系統 `#78272A`
  - 原料處理記錄及分析 `#2D6A4F`
- 其他系統的 `color` 留空，自動使用 `#3F4A56`。
- 鮮紅、鮮黃、鮮綠保留給未來的「異常／警告／正常」狀態，**不可作為系統色**。
- 風格：簡潔專業、扁平設計，**不用漸層**、不用陰影堆疊。
- 「測試中」標籤：深灰 `#2C2C2A` 外框＋深灰字，不用黃色。

## systems.json 欄位
| 欄位 | 必填 | 說明 |
|---|---|---|
| `updated` | 是 | 清單最後更新日期，顯示在頁尾 |
| `categories` | 是 | 類別順序：製造 → 品保 → 工務 → 資材 |
| `name` | 是 | 系統名稱 |
| `category` | 是 | 必須是 `categories` 中的一個 |
| `users` | 是 | 主要使用者 |
| `status` | 是 | `上線`／`開發/測試中`／`計劃中` |
| `description` | 是 | 簡短說明（建議 40 字內） |
| `note` | 否 | 內部備註，不會顯示在畫面上 |
| `url` | 是 | 完整網址，以 `https://` 開頭 |
| `color` | 否 | 系統代表色 `#RRGGBB`，留空用預設色 |
| `icon` | 是 | 內建圖示名稱（見下方） |
| `visible` | 是 | `false` 則不顯示；`計劃中` 的系統一律 `false` |

顯示規則（寫在 `index.html`）：
- `visible: false`、`status: 計劃中`、或 `url` 不是 http(s) 開頭 → 不顯示。
- `status: 開發/測試中` → 卡片加「測試中」標籤。
- 點擊卡片以新分頁開啟（`target="_blank" rel="noopener noreferrer"`）。

## 內建圖示名稱
`chart` 長條圖、`trend` 趨勢線、`leaf` 葉子、`clock` 時鐘、`package` 箱子、`clipboard` 檢核板、
`shield` 盾牌、`search` 放大鏡、`wrench` 扳手、`warehouse` 倉庫、`truck` 貨車、`users` 人員、
`file` 文件、`grid` 方格（預設）。
新增圖示：在 `index.html` 的 `ICONS` 物件加一組 24x24 線條 SVG path，並同步更新此清單與 README。

## 預覽與檢查
- 預覽必須透過本機伺服器（`python -m http.server`），直接雙擊 index.html 會讀不到 systems.json。
- 連結檢查：`.github/workflows/check-links.yml`，修改 systems.json 推上 GitHub 時會自動執行，也可在 Actions 頁面手動執行。

## 部署
- 尚未決定（Cloudflare Pages 或 GitHub Pages）。未經 Jackie 同意，不要新增任何部署設定。
