# You Only Look Once: Unified, Real-Time Object Detection

- **來源網址**：https://arxiv.org/abs/1506.02640
- **作者**：Joseph Redmon, Santosh Divvala, Ross Girshick, Ali Farhadi
- **機構**：University of Washington, Allen Institute for AI, Facebook AI Research
- **發表會議與年份**：CVPR 2016 (arXiv:1506.02640)
- **文件類型**：學術論文原文整理（Immutable Source）

---

## 原始文獻核心內容摘要 (Raw Paper Digest)

### 1. 核心理念與統一框架 (Unified Detection as Single Regression)
- **演進動機**：
  - 過去的物件偵測（如 DPM、R-CNN 家族）本質上是「多階段管線」：先產生候選框、再跑分類器、再微調座標、再執行 NMS 後處理。每個階段獨立訓練，速度緩慢難以優化。
  - YOLO 將物件偵測重新定義為**單一迴歸問題（Single Regression Problem）**：直接從原始像素端到端預測邊界框座標與類別機率，只看影像一次（You Only Look Once）。
- **推論速度革命**：
  - 標準 YOLO 在 Titan X GPU 上達到 **45 FPS**（畫面延遲僅 22 ms），mAP 達 63.4%（PASCAL VOC 2007）。
  - 輕量化版本 **Fast YOLO**（卷積層由 24 層精簡至 9 層）達到 **155 FPS**，mAP 達 52.7%。

---

### 2. 核心機制：S × S 網格劃分 (Grid Design)
- **網格劃分機制**：
  - 將輸入影像（$448 \times 448$）劃分為 $S \times S$ 個網格（論文中設定 $S = 7$）。
  - **責任指派原則**：如果某個物體的中心點（Center）落在某個網格內，則該網格就全權負責偵測該物體。
- **預測輸出維度（$S \times S \times (B \times 5 + C)$）**：
  - 每個網格預測 $B$ 個邊界框（Bounding Boxes，論文中 $B = 2$）。
  - 每個邊界框包含 5 個預測數值：$(x, y, w, h, \text{confidence})$：
    - $(x, y)$：物體中心相對於該網格邊界的相對位移（數值範圍 $0 \sim 1$）。
    - $(w, h)$：預測框相對於整張影像的寬高比例（數值範圍 $0 \sim 1$）。
    - $\text{confidence}$：定義為 $\text{Pr}(\text{Object}) \times \text{IoU}_{\text{pred}}^{\text{truth}}$。若該網格無物體則為 0；若有物體則希望等於預測框與真值的 IoU。
  - 每個網格同時預測 $C$ 個類別的條件機率 $\text{Pr}(\text{Class}_i | \text{Object})$（PASCAL VOC 為 20 類）。
  - 最終輸出張量維度：$7 \times 7 \times (2 \times 5 + 20) = 7 \times 7 \times 30 = 1470$ 維。

---

### 3. 多部分均方和誤差損失函數 (Multi-Part SSE Loss)
YOLO 全程採用均方和誤差（Sum-Squared Error, SSE），並透過加權權重調和不同任務：
$$\begin{aligned}
\mathcal{L} &= \lambda_{\text{coord}} \sum_{i=0}^{S^2} \sum_{j=0}^{B} \mathbb{I}_{ij}^{\text{obj}} \left[ (x_i - \hat{x}_i)^2 + (y_i - \hat{y}_i)^2 \right] \\
&\quad + \lambda_{\text{coord}} \sum_{i=0}^{S^2} \sum_{j=0}^{B} \mathbb{I}_{ij}^{\text{obj}} \left[ (\sqrt{w_i} - \sqrt{\hat{w}_i})^2 + (\sqrt{h_i} - \sqrt{\hat{h}_i})^2 \right] \\
&\quad + \sum_{i=0}^{S^2} \sum_{j=0}^{B} \mathbb{I}_{ij}^{\text{obj}} (C_i - \hat{C}_i)^2 \\
&\quad + \lambda_{\text{noobj}} \sum_{i=0}^{S^2} \sum_{j=0}^{B} \mathbb{I}_{ij}^{\text{noobj}} (C_i - \hat{C}_i)^2 \\
&\quad + \sum_{i=0}^{S^2} \mathbb{I}_{i}^{\text{obj}} \sum_{c \in \text{classes}} (p_i(c) - \hat{p}_i(c))^2
\end{aligned}$$

- **關鍵設計巧思**：
  1. $\lambda_{\text{coord}} = 5$：提高座標定位損失權重，避免定位偏差。
  2. $\lambda_{\text{noobj}} = 0.5$：多數網格不含物體（背景），降低無物體網格的信心度損失權重，解決正負樣本失衡。
  3. $\sqrt{w}, \sqrt{h}$ 開根號：反映大框的小偏差對 IoU 影響小、小框的小偏差對 IoU 影響大的物理幾何特性。

---

### 4. 實驗數據與錯誤分析 (VOC 2007 Error Analysis)
原文第 4.2 節（Hoiem et al. 診斷工具）揭示了 YOLO 與 Fast R-CNN 的根本特性差異：
1. **背景錯誤（Background Error / 虛警誤檢）**：
   - Fast R-CNN 背景誤檢率達 **13.6%**（候選框演算法容易把背景雜訊當成物體）。
   - YOLO 背景誤檢率僅 **4.75%**！原因：YOLO 在全圖進行全域推理（Global Context），能看清全圖背景環境，大幅減少背景「指鹿為馬」的錯誤。
2. **定位錯誤（Localization Error）**：
   - Fast R-CNN 定位錯誤僅 **8.6%**。
   - YOLO 定位錯誤高達 **19.0%**，是 YOLO 最主要的失分來源。
3. **模型集成（Ensemble）**：
   - 將 Fast R-CNN 與 YOLO 結合（利用 YOLO 降低背景誤檢），VOC 2007 mAP 從 71.8% 提升至 **75.0%**（提升 3.2%）。

---

### 5. 局限性 (Limitations)
1. **空間約束過緊**：每個網格只能預測**一個類別**與最多**兩個框**。對密集排列的小物體（如一群鳥、密集成群的行人）極易漏檢。
2. **泛化幾何形狀能力有限**：對未曾見過的長寬比或非典型角度物體適應力弱。
3. **粗粒度特徵**：特徵圖經多次下採樣（縮減 64 倍），小物體特徵損失嚴重。
