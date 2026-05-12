# my-site

FC 的公開靜態網站，部署於 GitHub Pages。

> **公開網址**：https://FFC225.github.io/my-site/

---

## 用途

收集 vibe coding 產出的、**可公開分享**的 HTML 頁面（小工具、活動資訊、家庭分享）。
任何沒裝 Tailscale 的朋友/家人都能用一般網路存取。

## 對內 vs 對外（重要）

FC 的靜態頁面採**兩套並存**架構：

| 項目 | 對內版（私人） | 對外版（公開，本 repo） |
|---|---|---|
| 部署 | 私有環境 | GitHub Pages |
| 內容 | 含敏感資料、需要互動 API | 整理過、確認可公開 |
| 可上傳 | 是 | 否（純靜態 + Git push） |

新增頁面前**先想清楚**這頁屬於對內還對外，避免把含 NAS IP / API key / 個人資料的內容誤推。詳見 [WORKFLOW.md](WORKFLOW.md)。

---

## 資料夾結構

```
my-site/
├── index.html                ← 主頁（卡片式目錄）
├── assets/
│   └── style.css             ← 共用樣式（森林色系 tokens）
├── README.md
├── WORKFLOW.md               ← 新增頁面 SOP
└── .gitignore
```

**規劃中的分類資料夾**（有內容時再建）：

| 資料夾 | 用途範例 |
|---|---|
| `weather/` | 攝影、戶外活動相關天氣工具 |
| `camping/` | 露營活動的菜單、食材、行程 |
| `health/` | 公開版本的家庭健康參考工具 |
| `tools/` | 小工具雜項 |

---

## 設計風格

森林色系（深綠主背景 `#1e2a1e`、米白文字 `#e8e6dc`、霧綠強調 `#a8c995`）。
所有頁面共用 `assets/style.css` 的 CSS variables，未來換主題只需改一處。

## 部署方式

```
git add <檔案>
git commit -m "<說明>"
git push
```

push 後 1-5 分鐘 GitHub Pages 自動部署。Source 設定為 `main` branch root（Settings → Pages）。

## 技術棧

純靜態 HTML/CSS，無 build step，無框架依賴。最簡單就是最穩。
