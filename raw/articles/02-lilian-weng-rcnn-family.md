# Object Detection for Dummies Part 3: R-CNN Family

- **來源網址**：https://lilianweng.github.io/posts/2017-12-31-object-recognition-part-3/
- **作者**：Lilian Weng (Lil'Log)
- **發布日期**：2017-12-31（後續持續修訂）
- **文件類型**：權威技術教學部落格（Immutable Source）

---

## 原始文獻核心內容摘要 (Raw Content Digest)

### 1. R-CNN (Region-based Convolutional Neural Network)
- **發表作者與年份**：Ross Girshick et al., CVPR 2014
- **核心流程**：
  1. **Region Proposal（候選區域生成）**：使用傳統無監督分割演算法 **Selective Search**，每張影像提出約 2,000 個候選區域（RoIs）。
  2. **Warping（影像幾何變形）**：不顧原始長寬比，強行將每個候選框裁剪並縮放（Warping）為固定大小（如 $227 \times 227$），以適配 CNN（AlexNet）的固定全連接層輸入。
  3. **Feature Extraction（特徵抽取）**：每個變形後的候選區域獨立送入一次 CNN，抽取 4,096 維的特徵向量（每張圖需進行 2,000 次 CNN 前向傳播！）。
  4. **Classification（分類）**：針對每個類別訓練獨立的線性 SVM 進行二分類。
  5. **Bounding Box Regression（邊界框迴歸）**：使用 Ridge Regression 訓練線性回歸模型，修正候選框的中心點與長寬座標 $(\Delta x, \Delta y, \Delta w, \Delta h)$。
  6. **Post-processing（後處理）**：採用 Non-Maximum Suppression (NMS) 依 IoU 門檻過濾高度重疊的冗餘預測框。
- **核心致命瓶頸（Pain-points）**：
  - 訓練極度繁瑣且耗時：分為 CNN fine-tuning、SVM 訓練、邊界框迴歸三階段，特徵需快取至硬碟（需數百 GB 磁碟空間）。
  - 推論極慢：在 GPU（如 K40）上每張影像推論耗時約 47 秒（主要花在 2,000 次重複的 CNN 運算）。
  - Warping 導致特徵形變失真。

---

### 2. Fast R-CNN
- **發表作者與年份**：Ross Girshick, ICCV 2015
- **核心演進動機**：為什麼要讓 2,000 個重疊的候選區域各自重複跑一次 CNN？為什麼不讓整張圖只跑一次 CNN，共享特徵圖？
- **核心架構突破**：
  1. **Shared Convolutional Feature Map（共享特徵圖）**：整張原始影像僅進行**一次** CNN 前向運算，生成全圖共享的特徵圖。
  2. **RoI Pooling（候選區域池化）**：
     - 將 Selective Search 提出的候選框直接投影映射到共享特徵圖上。
     - 將任意尺寸的特徵區域劃分成固定網格（如 $7 \times 7$），在每個網格子區塊執行 Max Pooling。
     - 徹底解決輸入尺寸不固定的問題，免除了原始影像 Warping 的幾何失真。
  3. **End-to-End Multi-task Loss（多任務聯合損失）**：
     - 捨棄獨立的 SVM 與離線回歸器，在網路末端同時預測分類（Softmax）與座標偏移量。
     - 引入 **Smooth L1 Loss** 進行邊界框迴歸：
       $$\text{smooth}_{L_1}(x) = \begin{cases} 0.5x^2 & \text{if } |x| < 1 \\ |x| - 0.5 & \text{otherwise} \end{cases}$$
       在誤差小時為 L2（梯度穩定平滑），在誤差大時為 L1（對離群值雜訊不敏感，避免梯度爆炸）。
- **性能數據與殘存瓶頸**：
  - 訓練速度較 R-CNN 提升 9 倍；純網路推論時間大幅縮短至 0.32 秒/張。
  - **殘存瓶頸（The Proposal Bottleneck）**：雖然 CNN 特徵計算只要 0.32 秒，但前面的 **Selective Search 仍需在 CPU 上耗時約 2.0~2.3 秒**！候選框生成演算法成為系統最嚴重的速度瓶頸。

---

### 3. Faster R-CNN
- **發表作者與年份**：Shaoqing Ren, Kaiming He, Ross Girshick, Jian Sun, NIPS 2015
- **核心演進動機**：徹底拋棄非神經網絡的慢速演算法（Selective Search），讓候選框的生成也交由神經網路端到端自主學習！
- **核心突破機制**：
  1. **RPN (Region Proposal Network，候選區域網絡)**：
     - 在 Backbone 輸出的共享特徵圖上滑動一個小卷積核（如 $3 \times 3$）。
     - 每個滑窗位置預設 $k$ 個多尺度、多長寬比的參照模板框——**Anchor Boxes**（典型為 3 種尺度 $\times$ 3 種長寬比 = 9 個錨框）。
     - 同步輸出 $2k$ 個 objectness 分數（前景 vs 背景）與 $4k$ 個邊界框座標偏移預測。
  2. **正負樣本分配規則（Anchor Matching Rules）**：
     - 正樣本（前景）：與某個 Ground Truth (GT) 框具有最高 IoU 的 Anchor，或 IoU > 0.7 的 Anchor。
     - 負樣本（背景）：與所有 GT 框的 IoU 均 < 0.3 的 Anchor。
     - 介於 0.3 到 0.7 之間的 Anchor 忽略不計入損失。
  3. **訓練策略（4-Step Alternating Training）**：
     - 輪流微調 RPN 與 Fast R-CNN 檢測頭，最終共享卷積層權重（後續亦可簡化為聯合端到端訓練）。
- **性能躍升**：
  - RPN 生成候選框僅耗時約 10 ms！
  - 整體推論時間降至約 0.2 秒（約 5 FPS），PASCAL VOC 2007 mAP 達到 73.2%，成為 Two-stage 偵測器的里程碑基石。

---

### 4. Mask R-CNN
- **發表作者與年份**：Kaiming He, Georgia Gkioxari, Piotr Dollár, Ross Girshick, ICCV 2017
- **核心突破**：
  1. **RoIAlign 取代 RoIPool**：
     - RoIPool 存在兩次粗暴的「座標向下取整（Quantization）」，例如除以採樣步長 16 後取整，造成空間座標微小偏移（Misalignment）。
     - RoIAlign 完全保留浮點數連續座標，採用**雙線性插值（Bilinear Interpolation）**計算精確特徵，徹底解決空間失準問題。
  2. **實例分割擴展（Instance Segmentation Branch）**：
     - 在原有分類與回歸分支旁，並行新增一個全卷積（FCN）分支，為每個 RoI 輸出二值遮罩（Binary Mask）。
