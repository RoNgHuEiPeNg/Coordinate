# 座標偵測與轉換工具

> 離線版 — WGS84 / GCJ02 / BD09 座標系統互轉、距離計算、地圖內嵌顯示

## 簡介

這是一個**純離線**的單頁式 HTML 網頁工具，用於偵測 GPS 位置、轉換不同座標系統之間的座標，以及計算兩點之間的球面距離。所有座標轉換演算法皆內建於 JavaScript 中，**無需任何外部 API 或網路連線即可執行座標轉換計算**。

## 功能總覽

### 1. 位置偵測

- 透過瀏覽器 Geolocation API 取得 GPS 位置
- 同時顯示 **WGS84**、**GCJ02**、**BD09** 三種座標（原邏輯 + 新邏輯）
- 提供新舊邏輯轉換對比驗證表格
- 支援內嵌 Google 地圖（WGS84 座標）及百度地圖（BD09 座標）即時預覽
- 經緯度精度統一至小數點第 14 位

### 2. 輸入位置顯示地圖

- 支援三種座標類型輸入：
  - **WGS84 | Google 地圖**
  - **BD09 原邏輯 | 百度地圖**
  - **BD09 新邏輯 (Java 版) | 百度地圖**
- 輸入經緯度後，以動畫展開對應的內嵌地圖 iframe

### 3. 座標轉換

- 支援六種轉換方向：
  - WGS84 → BD09 / BD09 → WGS84
  - WGS84 → GCJ02 / GCJ02 → WGS84
  - GCJ02 → BD09 / BD09 → GCJ02
- 每次轉換同時執行原邏輯與新邏輯（Java 版），並自動產生對比表格
- 顯示兩套邏輯結果的經緯度差異與距離差異（公尺）

### 4. 距離計算

- 自行新增座標點（支援 WGS84 或 BD09 輸入）
- 可一鍵使用偵測到的 GPS 位置作為座標點
- 座標點同時儲存 WGS84、BD09（原邏輯）、BD09（新邏輯）三組座標
- 使用 **Haversine 公式**計算球面距離
- 結果以原邏輯 / 新邏輯雙欄對比呈現

### 5. BD09 新邏輯

- 基於 Java 版 `CoordinateTransformUtil` 完整移植
- 獨立轉換介面，支援六種轉換方向
- 自動與原邏輯對比，顯示差異說明

## 座標系統說明

| 座標系 | 說明 | 使用平台 |
|--------|------|----------|
| **WGS84** | GPS 衛星定位原始座標，國際標準 | Google Maps（海外版） |
| **GCJ02** | 中國國測局加偏座標（火星座標） | Google Maps（中國版）、高德地圖、騰訊地圖 |
| **BD09** | 百度在 GCJ02 基礎上二次加偏 | 百度地圖 |

### 轉換鏈

```
WGS84 (GPS 原始)
  │
  ▼  國測局加偏
GCJ02 (火星座標)
  │
  ▼  百度二次加偏
BD09 (百度座標)
```

## 新舊邏輯差異

本工具內建兩套座標轉換邏輯，方便交叉驗證：

| 項目 | 原邏輯 | 新邏輯 (Java 版) |
|------|--------|------------------|
| `out_of_china` 邊界 | lng: 73.66 ~ 135.05 / lat: 3.86 ~ 53.55 | lng: 72.004 ~ 137.8347 / lat: 0.8293 ~ 55.8271 |
| GCJ02 → WGS84 | 迭代法（10 次迭代，精度更高） | 單步反算法 `(lng*2-mglng, lat*2-mglat)` |
| 其餘轉換公式 | 相同 | 相同 |

新邏輯來源：[wandergis/coordtransform](https://github.com/wandergis/coordtransform) 的 Java 版本 `CoordinateTransformUtil`。

## 技術規格

| 項目 | 說明 |
|------|------|
| 檔案結構 | 單一 `index.html`，無外部依賴 |
| 座標精度 | 小數點第 14 位 |
| 距離計算 | Haversine 公式（球面距離），地球半徑 6,371,000 公尺 |
| 定位方式 | 瀏覽器 Geolocation API（需使用者授權） |
| 地圖嵌入 | Google Maps Embed / 百度地圖 Marker API（需網路） |
| RWD 支援 | 桌面左側選單 / 手機版漢堡選單 + 側邊滑出 |
| 瀏覽器相容 | Chrome、Edge、Firefox、Safari 等現代瀏覽器 |

## 使用方式

1. 直接以瀏覽器開啟 `index.html` 即可使用
2. 座標轉換功能完全離線運作
3. 位置偵測功能需允許瀏覽器取用定位權限
4. 地圖顯示功能需要網路連線（Google Maps / 百度地圖）

```
20260408_座標/
├── index.html    # 主程式（單頁應用）
└── README.md     # 本文件
```

## 參考資料

- [百度地圖 iOS SDK - 座標轉換](https://lbsyun.baidu.com/docs/ios?title=iossdk/guide/tool/coordinate)
- [百度地圖 iOS SDK - 距離計算](https://lbsyun.baidu.com/docs/ios?title=iossdk/guide/tool/calculation)
- [wandergis/coordtransform](https://github.com/wandergis/coordtransform) — 座標轉換演算法參考
- [CoordinateTransformUtil (Java)](https://github.com/wandergis/coordtransform) — 新邏輯移植來源

## 授權

本工具僅供學習與研究用途。

---

Design by Ronghuei Peng with Claude Code's assistance.
