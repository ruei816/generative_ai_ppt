# Oriented Bounding Box (OBB) Detection

- **來源網址**：https://docs.ultralytics.com/tasks/obb
- **作者 / 機構**：Ultralytics YOLO Documentation Team
- **發布年份**：2024
- **文件類型**：官方技術規範文件（Immutable Source）

---

## 原始文獻核心內容摘要 (Raw Content Digest)

### 1. 什麼是旋轉框物件偵測 (What is Oriented Bounding Box Detection?)
旋轉框物件偵測（Oriented Bounding Box Detection, 簡稱 OBB）是標準物件偵測（Horizontal Bounding Box, HBB）的進階延伸。
- **傳統水平框（HBB）**：預測框強制平行於影像的水平與垂直軸，由 4 個參數定義：中心座標與長寬 $(x, y, w, h)$。
- **旋轉框（OBB）**：在水平框基礎上額外預測旋轉角度，使得邊界框能緊密沿著物體的幾何軸線旋轉，由 5 個參數 $(x, y, w, h, r)$ 或 4 個頂點座標 $(x_1, y_1, x_2, y_2, x_3, y_4, x_4, y_4)$ 定義。

---

### 2. 為什麼需要 OBB？傳統水平框（HBB）的致命痛點
1. **背景雜訊過多（Background Clutter）**：
   - 航拍遙感影像中的船隻、飛機、跑道或傾斜長條狀物體，若使用水平框，框內超過 60%~80% 的面積都是海水、柏油跑道等無關背景，嚴重干擾特徵提取。
2. **密集排列下的 NMS 誤刪（NMS Mis-suppression）**：
   - 當多艘細長船隻或車輛以 45 度角密集並排停靠時，相鄰兩艘船各自的水平框會高度重疊（水平 IoU 常高達 0.7 以上）。
   - 標準 NMS 會將旁邊那艘真實的船誤認為重複預測而**直接刪除**，導致極高漏檢率。
3. **傾斜文字與工業零件定位**：
   - 自然場景傾斜招牌 OCR、旋轉傳送帶上的機械手臂抓取，均需要精確的角度資訊方能執行實體夾取。

---

### 3. 資料標註與座標表示格式 (Data Formats)
- **四頂點多邊形格式 (`xyxyxyxy`)**：
  - DOTA 資料集標準標註格式：$[class\_id, x_1, y_1, x_2, y_2, x_3, y_3, x_4, y_4]$，座標均正規化至 $0 \sim 1$ 區間。
- **五參數旋轉格式 (`xywhr`)**：
  - 預測輸出格式：$[x, y, w, h, r]$。
  - $(x, y)$：中心點座標。
  - $(w, h)$：長邊與短邊（通常採長邊定義 Long-edge convention）。
  - $r$：旋轉弧度（Radians）。

---

### 4. 角度週期對稱性難題與邊界問題 (Periodicity & Boundary Discontinuity)
- **週期對稱性**：
  - 矩形框旋轉 $180^\circ$（$\pi$ 弧度）後，幾何形狀與旋轉前完全重合。因此旋轉角具有 $\pi$ 週期性模運算特性。
- **邊界跳躍問題（Boundary Jump）**：
  - 若角度範圍定義為 $[-\pi/2, \pi/2)$，當物體在 $-89.9^\circ$ 與 $+89.9^\circ$ 之間微小晃動時，數值差距高達 $179.8^\circ$。若直接計算數值誤差，會產生劇烈的梯度暴增與損失跳躍。
- **Ultralytics 規範**：
  - 內部採用長邊長度與歸一化定義，角度 $r$ 約束在半開區間 $[-\pi/4, 3\pi/4)$（即 $-45^\circ \sim 135^\circ$），配合平滑旋轉損失函數解決邊界問題。

---

### 5. 旋轉非極大值抑制 (Rotated NMS)
- 傳統基於軸對齊的 IoU 公式無法用於斜向矩形。
- 必須使用**多邊形求交演算法（Sutherland-Hodgman 多邊形裁剪法）**計算兩個旋轉矩形的實際交集多邊形面積，得出 **Rotated IoU**。
- Rotated NMS 使得並排停泊的斜向船隻之間的 Rotated IoU 保持在極低水準（接近 0），徹底解決了傳統 NMS 密集誤刪的痛點。

---

### 6. 代表性基準與模型性能 (Benchmark on DOTA v1.0)
- **DOTA v1.0**：航空遙感領域公認的權威 OBB 評估基準（涵蓋飛機、船隻、油罐、橋樑、車輛等 15 類目標）。
- **YOLOv8-OBB 官方實測數據**：
  - **`yolov8n-obb`**：輸入尺寸 1024，DOTA v1.0 $\text{mAP}_{50}$ 達 **78.0%**，參數量僅 3.1 M，FLOPs 為 23.3 B。
