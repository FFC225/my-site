# WORKFLOW — my-site 維護 SOP

> 給未來的自己（或 AI 協作者）看的操作手冊。

---

## 一、新增一個對外頁面

### Step 1：確認「對外」OK
- [ ] 頁面內**完全沒有** NAS Tailscale IP（`100.120.215.73`）、區網 IP（`192.168.x.x`）、`localhost`
- [ ] **沒有** API key、token、密碼
- [ ] **沒有** 個人敏感資料（身體數據、用藥、薪資、私密筆記）
- [ ] **沒有** 依賴只在內網跑的 fetch / XHR（例如 `/api/upload`、`/api/list`）

若有上述任一項，這頁屬於「對內版」，應放回 `IOS_HTML/`。

### Step 2：放到正確分類資料夾
依內容歸類，例如：
- 天氣/攝影類 → `weather/`
- 露營/戶外 → `camping/<日期-地點>/`
- 健康類 → `health/`
- 雜項小工具 → `tools/`

**檔名與資料夾名一律用英文**（GitHub Pages 對中文路徑支援不完美），HTML 內容用繁體中文沒問題。

### Step 3：套用共用樣式
HTML 的 `<head>` 加上：

```html
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<link rel="stylesheet" href="/my-site/assets/style.css">
```

> 注意路徑前綴：因為 repo 名是 `my-site`，網址會是 `FFC225.github.io/my-site/...`，所以 CSS 引用走絕對路徑 `/my-site/assets/style.css`，否則子資料夾頁面會找不到。
>
> 如果未來改用 `FFC225.github.io` 當 repo 名（根域名），這個前綴要全部拿掉。

### Step 4：加導覽列
建議子頁面都加：

```html
<nav class="nav-bar">
  <a class="btn btn-secondary" href="/my-site/">🏠 回主頁</a>
</nav>
```

同主題多頁時加互相切換按鈕（例：餐點 ↔ 食材清單）。

### Step 5：更新主頁 `index.html`
取消註解卡片區（移除外圍 `<!-- -->`），複製卡片範本，改三處：

1. `<h2>` — 頁面標題
2. `<p>` — 一句話描述
3. `<a href="...">` — 連結

### Step 6：Commit + Push

```powershell
cd C:\Users\FC\Downloads\Antigravity\my-site
git status                                    # 確認沒有手滑
git add <新檔案> index.html
git commit -m "feat: 新增 <頁面名> 頁面"
git push
```

### Step 7：驗證
- 等 1-5 分鐘
- 瀏覽器開新網址確認 200
- iPhone 4G/5G（關 Wi-Fi 與 Tailscale）開一次，確認對「真的外人」可達

---

## 二、修改既有頁面

```powershell
git add <修改的檔案>
git commit -m "fix: <改了什麼>"
git push
```

注意：**頁面在主頁卡片連到的網址若改名，主頁卡片也要改**。

---

## 三、「對內 vs 對外」決策樹

```
這頁要不要丟給朋友看？
├── 不要 / 含敏感資料 / 需要上傳功能
│   └── → 放 IOS_HTML/，走 NAS Docker
│
└── 要，且全是公開可看的內容
    ├── 是否同時自己用？
    │   ├── 是 → 兩邊都放（IOS_HTML 一份 + my-site 一份，內容可微調）
    │   └── 否 → 只放 my-site
    └── 是否含 fetch 內網 API？
        └── 是 → 改用公開 API 或刪掉那段功能，再放 my-site
```

---

## 四、常見問題

### Q1：push 後網址 404
- 等 5 分鐘（首次部署延遲）
- 檢查 `Settings → Pages` 的 Source 是 `main` / `root`
- 檢查 repo 是 Public（Private 免費版無 Pages）
- 確認檔案路徑大小寫（GitHub Pages 區分大小寫，Windows 不區分，本機過得了上線會 404）

### Q2：CSS 沒套用 / 樣式跑掉
- 檢查 `<link rel="stylesheet" href="...">` 路徑前綴是否 `/my-site/`
- 開 DevTools → Network，看 CSS 是不是 404
- 子資料夾頁面用相對路徑會壞掉，一律用絕對路徑 `/my-site/assets/...`

### Q3：中文檔名亂碼
- 檔名/資料夾名**改成英文**（這是規則，不是建議）
- HTML 內 `<meta charset="UTF-8">` 確認在最前面

### Q4：localStorage 資料消失 / 跨頁污染
- `FFC225.github.io` 整個帳號**所有 repo** 共用 localStorage origin
- key 一定要加 prefix：`my_site_<page>_<purpose>`
- 例：`my_site_camping_alishan_food_progress`

### Q5：Pages 部署超過 10 分鐘還 404
- 到 repo 的 `Actions` tab 看 deploy workflow 是否失敗
- 通常是 HTML 語法錯誤、引用了不存在的檔案

### Q6：commit 不小心包到敏感檔案
- **還沒 push**：`git reset HEAD~1`（保留改動）→ 編輯 `.gitignore` → 重新 commit
- **已 push**：用 `git filter-repo` 重寫歷史，或考慮 repo 全砍重來（公開 repo 上的歷史很難真正清乾淨）

---

## 五、給未來 AI 協作者的提醒

- 這個 repo 是 **public**——任何 commit 都會被全網看到
- 推之前先 `git diff --cached` 看一眼
- IOS_HTML 與 my-site 是**不同 repo**，不要混
- 主頁 `index.html` 占位區 (`.empty-state`) 在有內容後**整個刪掉**，不是註解掉
