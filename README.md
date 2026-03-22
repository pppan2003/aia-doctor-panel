# AIA 網絡醫生搜尋系統

> **只限 AIA 授權營業員使用。嚴禁發放予客戶。**

單頁 Web App，讓 AIA 授權營業員快速查詢全港網絡醫生資料，包括專科、診所地點、掛診醫院及地圖視圖。

---

## 功能

| 功能 | 說明 |
|------|------|
| 🔍 **全文搜尋** | 按醫生姓名或診所地址搜尋 |
| 🏥 **專科篩選** | 下拉選單篩選任何專科 |
| 📍 **地區篩選** | 按香港地區篩選診所 |
| 💊 **計劃篩選** | 睿選 / 特級健康之寶 & 自願醫保 / 至尊醫療 / 友心意 |
| 🏨 **醫院篩選** | 可組合篩選指定醫院入院權限 |
| ⚕️ **只限疣** | 獨立顯示只提供疣相關治療的醫生 |
| 🗺️ **地圖視圖** | 切換 LIST / MAP 模式，在 Leaflet 地圖上顯示診所位置 |
| 📱 **響應式設計** | 桌面版 + 手機版，iPhone / Android 均適用 |
| 🔐 **密碼保護** | 存取前須輸入授權密碼 |

---

## 資料說明

- **資料來源**：AIA Merged Panel Provider List PDF（2026 年 1 月 31 日版本）
- **醫生人數**：648 位
- **診所記錄**：1,012 筆
- **座標化**：所有診所地址已透過 [香港地理資料共享平台](https://geodata.gov.hk) 轉換為 WGS84 座標

---

## 技術架構

- **純靜態單頁應用**：單一 `index.html`，無需後端
- **地圖**：[Leaflet.js](https://leafletjs.com) + [MarkerCluster](https://github.com/Leaflet/Leaflet.markercluster)，CartoDB 底圖，無需 Google Maps API Key
- **樣式**：Tailwind CSS + Inter 字體
- **資料嵌入**：`DOCTORS_DATA` JSON 直接內嵌於 HTML

---

## PDF 資料更新流程

### 1. 安裝依賴

```bash
pip3 install pdfplumber pyproj
```

### 2. 從 PDF 提取醫生資料

```bash
python3 extract_pdf.py \
  --pdf "Merged Panel Provider List as of YYYY-MM-DD.pdf" \
  --output doctors_data.json
```

### 3. 地址座標化（Geocoding）

```bash
python3 geocode_addresses.py \
  --input doctors_data.json \
  --output doctors_geo.json
```

- 進度自動儲存至 `geocode_cache.json`，中斷後可續跑
- 使用 [HK GeoData API](https://geodata.gov.hk/gs/api/v1.0.0/locationSearch)，免費，無需 API Key

### 4. 將資料嵌入 HTML

將 `doctors_geo.json` 的內容（JSON.stringify 壓縮）替換 `index.html` 內的 `DOCTORS_DATA = [...]`。

---

## 存取密碼

存取密碼由管理員另行通知授權營業員，**不會記錄於本 Repo**。

密碼驗證採用前端 sessionStorage，**不具備伺服器端安全保護**，僅作基本防護用途。

---

## GitHub Pages 部署

本 Repo 設定為 **GitHub Pages** 靜態部署，push 至 `main` 後自動生效。

---

## 版本記錄

| 版本 | 日期 | 說明 |
|------|------|------|
| v1.3.0 | 2026-03-23 | iPhone UX 修正、footer 固定底部、手機地圖單一切換按鈕 |
| v1.2.1 | 2026-03-23 | 地圖佈局修正、13 個錯誤座標校正 |
| v1.2.0 | 2026-03-23 | Leaflet 地圖視圖、全港診所座標化 |
| v1.1.0 | 2026-03-23 | 只限疣醫生修正、多診所合併顯示 |
| v1.0.0 | 2026-03-23 | 首版發佈 |

---

## 免責聲明

本工具僅供 AIA 授權營業員內部查詢用途，資料以 AIA 官方名冊為準，如有差異請以官方版本為依據。
