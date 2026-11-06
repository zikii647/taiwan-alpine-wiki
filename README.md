# 🏔️ 山徑百岳誌 Taiwan Alpine Wiki

> **115-1 生成式 AI 第九週作業：建立自己的 `AGENTS.md` 與可擴充知識庫網站**
> - **作者**：政大 資訊四 劉子齊（112703008）
> - **GitHub Repository**：[https://github.com/zikii647/taiwan-alpine-wiki](https://github.com/zikii647/taiwan-alpine-wiki)
> - **線上瀏覽網址 (GitHub Pages)**：[https://zikii647.github.io/taiwan-alpine-wiki/](https://zikii647.github.io/taiwan-alpine-wiki/)

## 專案簡介

**山徑百岳誌 (Taiwan Alpine Wiki)** 是一個以「山系森林風」設計的台灣高山百岳與縱走路線知識庫網站。不同於單次性的 Vibe Coding 網頁，本專案透過 [`AGENTS.md`](./AGENTS.md) 建立了嚴謹的**視覺設計系統（Design Tokens）**、**資料與視圖分離架構（Data-Driven Schema）**以及**AI 代理標準作業流程（SOP）**。

網站目前收錄五大經典百岳路線與實戰紀錄：
1. **北二段：閂山、鈴鳴山縱走（3天2夜）** — 結合 2026-09-25 實際核准入園行程（730 林道 11.7K 起登、夜宿 25K 工寮，走訪百岳 #79 [閂山](https://hiking.biji.co/index.php?q=review&act=info&review_id=10875) 3,168m 與百岳 #54 鈴鳴山 3,272m）。
2. **玉山群峰線：玉山主峰、西峰（2天1夜）** — 經塔塔加登山口與[排雲山莊](https://zh.wikipedia.org/zh-tw/%E6%8E%92%E9%9B%B2%E5%B1%B1%E8%8E%8A)（3,402m）登頂台灣第一高峰[玉山主峰](https://www.ysnp.gov.tw/Trail/7fa5c242-df1a-4a8e-bcab-32dc55b1f7b6)（3,952m）。
3. **雪霸聖稜起點：雪山主峰、東峰（3天2夜）** — 經七卡山莊、哭坡、三六九臨時營地、黑森林與圈谷登頂[雪山主峰](https://hiking.biji.co/index.php?q=trail&act=detail&id=431)（3,886m）。
4. **中央山脈南二段：嘉明湖、向陽山、三叉山（3天2夜）** — 走訪[嘉明湖國家步道](https://recreation.forest.gov.tw/Trail/RT?tr_id=139)、向陽山（3,603m）與[三叉山](https://zh.wikipedia.org/zh-tw/%E4%B8%89%E5%8F%89%E5%B1%B1)（3,496m）。
5. **能高越嶺西段：奇萊南峰、南華山（2天1夜）** — 循能高越嶺古道西段夜宿天池山莊（2,860m），登頂[奇萊南峰與南華山](https://hiking.biji.co/index.php?q=trail&act=detail&id=430)。

## 專案結構

```text
taiwan-alpine-wiki/
├── index.html          # 網站主入口（含百岳路線百科、縱走日誌、互動裝備檢查表、入園指南與 AI 路線產生器）
├── AGENTS.md           # AI 開發與維護規則手冊（定義視覺 Token、ROUTES_DATA Schema 與驗收標準）
└── README.md           # 專案說明文件
```

## 如何使用 `AGENTS.md` 讓 AI 持續擴充網站？

在未來的對話中，只需請 AI 先讀取 [`AGENTS.md`](./AGENTS.md)，即可依據內建的 `ROUTES_DATA` 18 欄位 Schema 與山系森林風色票（深松針綠 `#1B3B2B`、高山苔蘚綠 `#2E5A44`、晨霧米白 `#F7F5F0`、曙光杉木橘 `#C86D3B`）快速新增百岳路線或功能模組，而不會破壞既有版面與互動邏輯。
