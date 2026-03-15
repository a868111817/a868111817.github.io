# ~/ai-notes

一個仿終端機風格的靜態部落格，用來記錄 AI 學習筆記與技術心得。

## 專案架構

```
a868111817.github.io/
├── index.html          # 單頁應用主體（所有 UI、邏輯皆在此）
├── articles/
│   ├── index.json      # 文章清單（metadata）
│   ├── about.md        # 關於頁面
│   └── *.md            # 各篇文章（Markdown 格式）
└── .nojekyll           # 告訴 GitHub Pages 跳過 Jekyll 處理
```

### 技術棧

- 純靜態 HTML / CSS / JavaScript，無框架、無建置工具
- [marked.js](https://cdn.jsdelivr.net/npm/marked/marked.min.js) — 瀏覽器端 Markdown 渲染
- [JetBrains Mono](https://fonts.google.com/specimen/JetBrains+Mono) — 等寬字型
- GitHub Pages — 免費靜態網站托管

### 運作流程

1. 頁面載入時，fetch `articles/index.json` 取得所有文章 metadata
2. 點擊文章或輸入指令，動態 fetch 對應的 `.md` 檔並用 marked.js 渲染
3. 所有路由、渲染邏輯都在 `index.html` 的 `<script>` 中完成

---

## 新增文章

**步驟一：** 在 `articles/` 目錄新增 Markdown 檔，例如 `articles/my-article.md`

```markdown
# 文章標題

文章內容...
```

**步驟二：** 在 `articles/index.json` 新增一筆 metadata

```json
[
  {
    "id": "my-article",
    "date": "2026-03-15",
    "words": 800,
    "tags": ["AI", "筆記"],
    "title": "文章標題"
  }
]
```

> `id` 必須與檔名相同（不含 `.md`）。文章清單依陣列順序顯示，最新的放最前面。

---

## 本地端測試

因為網頁用 `fetch()` 載入檔案，必須透過 HTTP 伺服器才能正常運作（直接開啟 `index.html` 會被瀏覽器 CORS 限制）。

**方法一：Python（推薦，不需安裝）**

```bash
python -m http.server 8080
```

**方法二：Node.js**

```bash
npx serve .
```

**方法三：VS Code Live Server**

安裝 Live Server 擴充套件，右鍵點擊 `index.html` 選 **Open with Live Server**。

啟動後開啟 `http://localhost:8080` 即可預覽。

---

## 終端機指令

網頁底部有一個仿 Bash 的輸入框，支援以下指令：

| 指令 | 說明 |
|------|------|
| `ls` / `list` | 列出所有文章 |
| `cat <id>` | 閱讀指定文章 |
| `about` | 關於頁面 |
| `home` | 回到首頁 |
| `clear` | 清除畫面 |
| `help` | 顯示說明 |

支援方向鍵 `↑` / `↓` 瀏覽歷史指令，`Tab` 鍵自動補全。

---

## 部署

推送到 GitHub 後，GitHub Pages 會自動從 `main` branch 部署。`.nojekyll` 檔案確保 GitHub 直接提供靜態檔案，不經過 Jekyll 處理。
