# AGENTS.md — 山徑百岳誌 (Taiwan Alpine Wiki) AI 開發與維護規範手冊

> **給 AI Agent 的首要指令**：每次開始修改本專案、新增百岳路線或擴充功能前，請務必完整閱讀本文件。本文件定義了專案目的、山系森林風視覺設計系統、檔案結構、路線資料 Schema，以及「要什麼成品、怎麼算做完、有什麼限制」三大交辦原則。

## 1. 專案目的與核心定位 (Project Purpose)

**山徑百岳誌 (Taiwan Alpine Wiki)** 是一個專為台灣高山百岳與縱走健行者打造的結構化知識庫網站。
本專案旨在解決登山資訊散落於各處的痛點，將台灣經典百岳路線（如北二段[閂山鈴鳴山](https://hiking.biji.co/index.php?q=review&act=info&review_id=10875)、[玉山主西峰](https://www.ysnp.gov.tw/Trail/7fa5c242-df1a-4a8e-bcab-32dc55b1f7b6)、[雪山主東峰](https://hiking.biji.co/index.php?q=trail&act=detail&id=431)、[嘉明湖向陽三叉山](https://recreation.forest.gov.tw/Trail/RT?tr_id=139)、[奇萊南華](https://hiking.biji.co/index.php?q=trail&act=detail&id=430)）的**海拔里程、宿營水源、每日行程、入園／入山證規範與裝備檢查表**整合為單一且易於擴充的靜態網站，並可直接部署於 GitHub Pages。

核心目標：
- **資料與視圖分離 (Data-Driven Rendering)**：所有百岳路線與裝備清單皆儲存於結構化 JavaScript 資料陣列（`ROUTES_DATA` 與 `GEAR_CHECKLIST_DATA`）中，讓 AI 日後新增路線時只需擴充資料物件，無須重寫 HTML 版面。
- **山系森林風體驗 (Alpine & Forest Aesthetics)**：提供沉穩、高對比且適配手機與桌機的戶外誌視覺體驗。
- **100% 事實與來源錨定 (Strict Source Grounding)**：每筆百岳海拔、里程與許可證資訊皆須附上官方機構（如[臺灣登山申請一站式服務網](https://hike.taiwan.gov.tw/)、[林業及自然保育署](https://www.forest.gov.tw/0000345)、[交通部中央氣象署登山氣象](https://www.cwa.gov.tw/V8/C/L/Mountain/Mountain.html?PID=D093)）或權威路線資料庫之可點擊連結。

## 2. 網頁整體風格與設計系統 (Visual Style & Design System)

本專案採 **「山系森林風 (Alpine Forest & Earth Palette)」** 設計語言，融合深松綠、苔蘚綠、岩砂米與暖杉木色調，營造台灣中高海拔針葉林與高山箭竹草原的視覺層次。

### 2.1 色彩變數規範 (Color Tokens)
AI 在撰寫或修改元件樣式時，必須嚴格使用以下色彩配置，確保全站正文對比度符合 WCAG AA (≥ 4.5:1) 標準：

| 語意角色 | 色票名稱 | Hex 色碼 | 適用元件與範圍 |
| :--- | :--- | :--- | :--- |
| 主背景色 (Surface Primary) | 晨霧米白 (Mist Cream) | `#F7F5F0` | 全頁背景、主內容區底色 |
| 次級背景 (Surface Secondary) | 岩砂淺米 (Sandstone Beige) | `#EFECE4` | 篩選器區塊、表格交替列、知識卡片內嵌背景 |
| 卡片基底 (Card Surface) | 純淨山嵐白 (Alpine White) | `#FFFFFF` | 路線卡片、彈跳視窗、檢查表容器 |
| 主品牌色 (Primary Forest) | 深松針綠 (Deep Pine Green) | `#1B3B2B` | 導覽列、主標題、主要按鈕、重點強調區塊 |
| 次品牌色 (Secondary Moss) | 高山苔蘚綠 (Alpine Moss) | `#2E5A44` | 次級按鈕、徽章背景、圖示與懸停狀態 |
| 點綴強調色 (Accent Cedar) | 曙光杉木橘 (Sunrise Cedar) | `#C86D3B` | 難度警示、重要入園提醒、山屋/水源標籤、CTA 按鈕 |
| 主文字色 (Text Primary) | 玄武岩深灰 (Basalt Charcoal) | `#1F2924` | 內文、卡片標題、數據數值 |
| 次文字色 (Text Muted) | 冷杉灰綠 (Slate Pine) | `#526358` | 副標題、海拔標註、日期與來源說明 |
| 邊框線條 (Border Subtle) | 乾草灰米 (Bamboo Grass Border) | `#DCE0D9` | 卡片外框、分隔線、表單輸入框 |

### 2.2 排版與元件規範 (Typography & Components)
- **字體設定**：使用 `'Inter', 'Noto Sans TC', sans-serif` 作為主要介面字體；海拔高度、里程與百岳編號等數值統一加上 `tabular-nums`（等寬數字）確保對齊。
- **圓角與陰影**：主卡片統一使用 `rounded-2xl` (`16px`) 搭配柔和陰影與 `1px solid #DCE0D9` 細邊框；標籤徽章（Badges）使用 `rounded-full` (`px-3 py-1 text-xs font-semibold`)。
- **難度分級徽章顏色**：
  - `新手百岳 (Beginner)`：背景 `#E3F2EA`、文字 `#1B5E3A`
  - `中階縱走 (Intermediate)`：背景 `#FEF3C7`、文字 `#92400E`
  - `進階百岳 (Advanced)`：背景 `#FEE2E2`、文字 `#991B1B`

## 3. 專案目錄與檔案命名規則 (Directory Structure & File Naming)

為確保部署至 GitHub Pages 時路徑絕對穩定（Linux 伺服器區分大小寫），全專案嚴格遵守以下檔案命名與目錄規則：

```text
taiwan-alpine-wiki/
├── index.html          # 網站主入口（單頁多視圖架構，內含完整 CSS、資料陣列與互動邏輯）
├── AGENTS.md           # AI 代理開發與維護規範手冊（本文件）
├── README.md           # GitHub Repository 專案說明書與線上預覽導覽
└── assets/             # 靜態圖片與圖示資源資料夾（選用）
    └── hero-alpine.jpg # 首頁高山雲海橫幅插畫
```

### 檔案命名鐵則
1. **最外層入口**：網站主檔案必須命名為 `index.html` 並置於 Repository 根目錄 `/`（不可多包一層子資料夾）。
2. **純英文小寫與連字號 (`kebab-case`)**：所有新增檔案、圖片或獨立頁面一律使用英文小寫、數字與連字號（例如 `shuan-lingming.jpg`、`gear-guide.html`），**嚴禁使用中文檔名、空白字元或大寫字母**，避免在 GitHub Pages 產生網址亂碼或 404 破圖。
3. **無外部建置依賴**：為方便直接上傳 GitHub 即刻瀏覽，前端統一使用瀏覽器原生 HTML5 + Tailwind CSS CDN (`https://cdn.tailwindcss.com/3.4.17`) + 原生 Vanilla JavaScript，禁止引入需要 `npm run build` 的打包工具。

## 4. 頁面結構與五大核心模組 (Page Architecture)

`index.html` 採單頁多視圖（Single-Page Multi-View Tabs）結合互動彈窗架構，頂部設有固定導覽列，切換以下五個核心視圖區塊：

1. **百岳路線百科 (`#view-routes`)**：
   - 包含高山視覺 Hero 橫幅、百岳知識庫統計指標（收錄路線數、涵蓋百岳數、最高海拔 3,952m）。
   - 提供**區域篩選**（全部、北二段／太魯閣、玉山山脈、雪山山脈、中央山脈南段、中央山脈中段）、**難度篩選**與**關鍵字即時搜尋**。
   - 每張路線卡片顯示：路線名稱、所屬國家公園／管理處、難度分級、天數、總里程、最高海拔、百岳列表標籤，點擊「查看完整路況與行程」可開啟路線詳情 Modal（含每日行程表、水源宿營點、入園入山規定與官方來源連結）。
2. **縱走實戰日誌 (`#view-journal`)**：
   - 紀錄作者實際踏查之高山行程（首篇收錄 **2026-09-25 至 2026-09-27 太魯閣國家公園北二段「閂山鈴鳴山」三天兩夜縱走**，對應入園申請編號 `T115022642`），包含 730 林道里程節點海拔剖析圖、25K 工寮宿營與林道倒木路況筆記。
3. **百岳裝備檢查表 (`#view-gear`)**：
   - 依據[臺灣登山申請一站式服務網裝備檢查規範](https://hike.taiwan.gov.tw/notice_a10.aspx)設計的互動式裝備清單（分為「背負與睡眠系統」、「行進衣著與防雨」、「炊事飲水與行動糧」、「定位通訊與證件安全」四大類）。
   - 支援勾選進度百分比計算、依行程天數自動提示必備品項、一鍵重設與儲存進度至 `localStorage`。
4. **入園申請與山林禮儀 (`#view-permits`)**：
   - 整理台灣三大高山國家公園（玉山、太魯閣、雪霸）入園證、林業保育署山屋申請與[內政部警政署入山許可證](https://nv2.npa.gov.tw/NM107-604Client/)申辦流程對照表，以及山屋頭燈紅光模式、保鮮盒減塑降噪與 LNT 無痕山林守則。
5. **AI 擴充規範與路線產生器 (`#view-ai-rules`)**：
   - 視覺化展示本專案 `AGENTS.md` 的架構，並提供**「新增百岳路線 JSON 產生器」**：使用者或 AI 可在表單中輸入新路線資訊，即時預覽符合 Schema 的新路線卡片並一鍵複製標準 JSON 程式碼片段。

## 5. 新增內容時必須遵守的格式 (Data Schema for New Content)

當使用者要求「新增一條百岳路線」時，AI 必須在 `index.html` 的 `ROUTES_DATA` 陣列中新增一個符合以下 Schema 的物件，**不可遺漏任何必填欄位**：

```javascript
{
  id: "route-unique-slug",           // [必填] 唯一識別碼，一律使用英文小寫 kebab-case，如 "nan-hu-da-shan"
  name: "路線中文名稱",               // [必填] 例如 "北一段：南湖大山、中央尖山群峰"
  region: "所屬山脈或分段",           // [必填] 例如 "中央山脈北二段"、"玉山山脈"、"雪山山脈"
  park: "管轄國家公園或單位",         // [必填] 例如 "太魯閣國家公園"、"玉山國家公園"、"雪霸國家公園"、"林業保育署臺東分署"
  parkUrl: "https://...",            // [必填] 管轄單位或官方路線介紹網址（必須為真實可點擊之 https 連結）
  difficulty: "新手百岳",             // [必填] 僅限三種枚舉值："新手百岳" | "中階縱走" | "進階百岳"
  days: "3天2夜",                    // [必填] 建議行程天數，如 "2天1夜"、"3天2夜"
  distanceKm: 31.6,                  // [必填] 往返或全程總里程（數值，單位：公里）
  elevationM: 3272,                  // [必填] 路線最高點海拔（數值，單位：公尺）
  trailhead: "登山口名稱",            // [必填] 例如 "730林道 11.7K 柵欄 → 17.5K 登山口"
  peaks: [                           // [必填] 途經百岳清單（至少 1 座）
    { name: "鈴鳴山", elevation: 3272, rank: 54 },
    { name: "閂山", elevation: 3168, rank: 79 }
  ],
  permitRequired: "需申請國家公園入園證", // [必填] 入園證或山屋申請規定
  policePermit: "需申請警政署入山證",     // [必填] 入山證規定（"需申請警政署入山證" 或 "免辦入山證"）
  lodgeOrCamp: "25K工寮 / 營地",         // [必填] 住宿山屋或營地名稱
  waterSource: "24.5K水源處（須過濾煮沸）",// [必填] 沿途穩定水源說明
  summary: "80字以內的路線特色摘要...",    // [必填] 簡述地形特色、林相與看點
  itinerary: [                       // [必填] 每日行程陣列，每天包含 day 與 plan
    { day: "D1", plan: "起登點 → 中繼站 → 宿營點（含關鍵里程標記）" },
    { day: "D2", plan: "宿營點 → 三角點攻頂 → 返回宿營點或下山" }
  ],
  cautions: [                        // [必填] 至少 2 項安全注意事項
    "具體路況提醒（如咬人貓、碎石坡、崩壁或水源季節性變化）",
    "高山症預防與撤退折返點說明"
  ],
  sourceTitle: "外部參考資料標題",    // [必填] 例如 "健行筆記：閂山鈴鳴山路線資訊"
  sourceUrl: "https://..."           // [必填] 驗證該路線數據之具體外部網址
}
```

## 6. AI 修改或新增內容的守則 (AI Rules & Guardrails)

根據課堂 Agentic Work「好老闆三原則」，AI 執行任務時必須嚴格遵守：

### 6.1 要什麼成品 (Expected Deliverable)
- 每次修改後，必須輸出完整、可直接執行的 `index.html`（若新增了規則或改變目錄結構，需同步更新 `AGENTS.md` 與 `README.md`）。
- 嚴禁使用 `// ... existing code ...` 或 `TODO` 省略既有程式碼，確保使用者下載後可直接拖曳上傳至 GitHub Repository。

### 6.2 怎麼算做完 — 驗收標準 (Definition of Done)
1. **Schema 完整性檢查**：新增的路線物件必須包含第 5 節定義的所有 18 個必填欄位，且 `difficulty` 必須屬於三個合法枚舉值之一。
2. **自動連動驗證**：新增路線後，首頁的「收錄百岳路線數」統計卡片、區域篩選按鈕、關鍵字搜尋與路線詳情彈窗皆能自動讀取並正常顯示該路線，不需手動硬編碼 DOM。
3. **來源真實性檢查**：所有海拔高度、百岳排名、步道里程與入園規定皆有真實外部網址（`parkUrl`、`sourceUrl`）支撐，嚴禁捏造數據或使用 `#` 空連結。
4. **無錯誤執行**：頁面在手機（375px）與桌機（1440px）寬度下皆無水平捲軸溢位，所有按鈕與 Tab 切換皆有實際互動反應，且不使用瀏覽器原生 `alert()`、`confirm()` 或 `prompt()`。

### 6.3 有什麼限制 — 禁止事項 (Strict Constraints)
- **禁止破壞既有路線資料**：除非使用者明確要求刪除或修正，否則新增路線時必須完整保留 `ROUTES_DATA` 中既有的所有路線紀錄與 `2026-09-25 閂山鈴鳴山` 實戰日誌。
- **禁止更改核心色彩與版面架構**：不得擅自將山系森林風改為霓虹色、純黑背景或無樣式白底，亦不可移除頂部五個核心導覽分頁。
- **禁止引入外部建置工具或中文檔名**：所有新增資源必須符合英文小寫 `kebab-case`，且維持單檔或純靜態結構以相容 GitHub Pages。

## 7. 日後擴充的標準交辦指令範本 (Prompt Templates)

當專案未來需要新增路線或新功能時，只需在新對話中向 AI 下達以下指令：

### 範本 A：新增百岳路線
> 「請先閱讀專案中的 `AGENTS.md`。請幫我在 `index.html` 的 `ROUTES_DATA` 中新增一條『武陵四秀（品田山、池有山、桃山、喀拉業山）3天2夜』路線，查證雪霸國家公園官方海拔、里程、新達山屋與桃山山屋水源資訊並附上來源網址，完成後給我完整的 `index.html`。」

### 範本 B：新增功能區塊或新分頁
> 「請先閱讀 `AGENTS.md` 了解本專案的山系森林風設計系統與架構。請在導覽列新增一個『高山百岳天氣與路況速查』分頁，維持 `#1B3B2B` 深松綠與 `#F7F5F0` 晨霧米白配色，整合中央氣象署各百岳氣象連結，並確保不破壞現有的五個視圖與路線資料。」
