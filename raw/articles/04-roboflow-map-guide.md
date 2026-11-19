# What is Mean Average Precision (mAP) in Object Detection?

- **來源網址**：https://roboflow.com/blog/mean-average-precision
- **作者 / 機構**：Roboflow Blog (Computer Vision Educational Series)
- **發布年份**：2023-2024
- **文件類型**：權威技術教學指南（Immutable Source）

---

## 原始文獻核心內容摘要 (Raw Content Digest)

### 1. 為什麼傳統分類指標（Accuracy）無法評估物件偵測？
在傳統影像分類中，只需比較「預測標籤」是否等於「真實標籤」。但在物件偵測中，模型必須同時完成兩件事：
1. **分類（Classification）**：預測這是不是一輛車？
2. **定位（Localization）**：邊界框畫得夠不夠準？
此外，每張圖中可能包含未知數量的多個物體，甚至包含大面積的無物體背景。因此，必須建立一套綜合衡量「找得全不全（Recall）」與「找得準不準（Precision）」以及「框得緊不緊（IoU）」的評估指標。

---

### 2. 交併比 (Intersection over Union, IoU)
- **數學定義**：
  $$\text{IoU} = \frac{\text{Area of Overlap}}{\text{Area of Union}} = \frac{\text{Area}(B_{\text{pred}} \cap B_{\text{gt}})}{\text{Area}(B_{\text{pred}} \cup B_{\text{gt}})}$$
- **判斷正負樣本的門檻（Threshold $\alpha$）**：
  - **True Positive (TP, 真陽性)**：預測類別正確，且 $\text{IoU} \ge \alpha$（如 $\alpha = 0.5$）。
  - **False Positive (FP, 偽陽性 / 虛警)**：
    - 預測類別錯誤；或
    - 預測框落在純背景上（$\text{IoU} < 0.1$）；或
    - 預測框與真值框的 $\text{IoU} < \alpha$；或
    - **重複檢測**：若有多個預測框與同一個真實目標匹配，僅有**置信度最高的那一個**算作 TP，其餘所有重疊預測框一律嚴格計為 FP！
  - **False Negative (FN, 偽陰性 / 漏檢)**：存在真實目標（Ground Truth），但模型完全沒有預測出對應的檢測框，或者所有預測框的 $\text{IoU} < \alpha$。

---

### 3. 精確率 (Precision) 與召回率 (Recall)
- **精確率 (Precision)**：在模型**所有預測為有物體**的框中，有多少比例是真正正確的？
  $$\text{Precision} = \frac{\text{TP}}{\text{TP} + \text{FP}}$$
  - 核心價值：衡量模型「虛警率（誤報）」，Precision 高代表模型出手很謹慎，不會亂報。
- **召回率 (Recall)**：在畫面中**所有真正存在的目標**中，模型成功抓到了多少比例？
  $$\text{Recall} = \frac{\text{TP}}{\text{TP} + \text{FN}}$$
  - 核心價值：衡量模型「漏檢率」，Recall 高代表模型覆蓋全面，不會漏掉獵物。
- **置信度門檻（Confidence Threshold）的消長天平**：
  - 調高置信度門檻（如 0.8）：模型只抓極有把握的目標 $\to$ Precision 升高，但容易漏掉模糊目標，Recall 下降。
  - 調低置信度門檻（如 0.2）：模型寧可抓錯不可放過 $\to$ Recall 升高，但引來大量雜訊虛警，Precision 下降。

---

### 4. 平均精度 (Average Precision, AP) 與 P-R 曲線
- **P-R 曲線（Precision-Recall Curve）**：以 Recall 為橫軸、Precision 為縱軸繪製出的權衡曲線。
- **AP 定義**：P-R 曲線下的面積（Area Under Curve, AUC-PR）。
- **插值計算標準**：
  - **PASCAL VOC 2007 (11-point Interpolation)**：在 Recall = 0.0, 0.1, 0.2, ..., 1.0 共 11 個固定點採樣最大 Precision 並取平均值。
  - **PASCAL VOC 2010+ 及 COCO (All-point / 101-point Interpolation)**：在 0.00 到 1.00 每隔 0.01 採樣（101 點），以更精確的平滑數值積分計算 AUC。

---

### 5. 平均精度均值 (Mean Average Precision, mAP)
若資料集中有 $N$ 個類別（例如 VOC 20 類，COCO 80 類），將每個類別的 AP 分別計算後取算術平均：
$$\text{mAP} = \frac{1}{N} \sum_{i=1}^{N} \text{AP}_i$$

---

### 6. 現代黃金標準：mAP@50 與 mAP@50:95 (COCO 標準)
- **mAP@50 (或 mAP@0.5)**：
  - 固定在 $\text{IoU} = 0.50$ 的單一寬鬆門檻下計算。只要預測框與真值重疊達 50% 就視為命中。
  - 適合檢驗模型「有沒有找到東西」。
- **mAP@50:95 (COCO 主指標，常簡寫為 mAP 或 AP)**：
  - 在 10 個不同的 IoU 門檻下分別計算 mAP：$\{0.50, 0.55, 0.60, 0.65, 0.70, 0.75, 0.80, 0.85, 0.90, 0.95\}$，然後取其平均值。
  - **核心價值**：大幅獎勵「邊界框緊密貼合目標」的模型。一個框框畫得很鬆垮、歪斜的模型，在 mAP@50 也許能拿高分，但在 mAP@75 與 mAP@90 會全軍覆沒，導致 mAP@50:95 嚴重失分！
- **COCO 尺度細分指標**：
  - $\text{mAP}_S$：面積小於 $32^2$ 像素的小目標。
  - $\text{mAP}_M$：面積介於 $32^2 \sim 96^2$ 的中目標。
  - $\text{mAP}_L$：面積大於 $96^2$ 的大目標。
