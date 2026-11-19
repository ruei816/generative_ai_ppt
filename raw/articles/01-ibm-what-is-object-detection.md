# What is Object Detection?

- **來源網址**：https://www.ibm.com/think/topics/object-detection
- **作者 / 機構**：Jacob Murel, Ph.D. & Eda Kavlakoglu | IBM Research / IBM Think
- **發布年份**：2024
- **文件類型**：企業級技術百科（Immutable Source）

---

## 原始文獻核心內容摘要 (Raw Content Digest)

### 1. 什麼是物件偵測 (What is Object Detection?)
物件偵測（Object Detection）是電腦視覺（Computer Vision）與人工智慧（Artificial Intelligence）的核心技術之一，利用神經網絡（Neural Networks）在數位影像或視訊中「定位（Locate）」並「分類（Classify）」物體。

物件偵測本質上是兩個子任務的結合：
1. **物件定位（Object Localization）**：確定影像中某個物體所處的實體空間位置，通常透過預測邊界框（Bounding Box）來標記。
2. **物件分類（Object Classification）**：判定該邊界框內的物體屬於哪一個語義類別（例如：人、汽車、交通標誌、狗）。

### 2. 電腦視覺任務的三大層次比較 (Comparison of Computer Vision Tasks)
- **影像分類（Image Classification / Recognition）**：
  - 任務目標：對「整張影像」賦予單一標籤。
  - 特點：只回答「這張圖有沒有貓？」，不關心貓在畫面的哪裡、也不關心圖裡有幾隻貓。
- **物件定位（Object Localization）**：
  - 任務目標：針對影像中的「單一主要物體」找出其所在位置並標註邊界框。
  - 特點：通常假設圖中只有一個主要主體。
- **物件偵測（Object Detection）**：
  - 任務目標：在同一張影像中，同時偵測出「多個不同類別、多個實例（Multi-instance）」的物體，並分別給予各自的邊界框與類別機率。
- **影像分割（Image Segmentation）**：
  - 任務目標：提供像素等級（Pixel-level）的邊界輪廓。
  - 特點：比邊界框更精確，描繪出物體的精確不規則幾何形狀（分語義分割 Semantic Segmentation 與實例分割 Instance Segmentation）。

### 3. 電腦如何「看見」世界？（影像的數位化與特徵提取）
- **取樣與量化（Sampling & Quantization）**：
  - 自然界的光影是連續的類比訊號，電腦將其轉換為由像素（Pixel）組成的離散二維網格矩陣（Grid of Pixels）。
  - 彩色影像通常由三通道（RGB）構成，每個像素點具有 0~255 的數值。
- **模式識別（Pattern Recognition vs. Human Vision）**：
  - 電腦並不像人類那樣具有對物體的常識理解；深度學習模型是透過分析像素數值，識別局部視覺特徵（邊緣、線條、顏色對比、紋理），並逐步組合成高階抽象特徵（形狀、部件、完整輪廓）。

### 4. 現代物件偵測的通用模組管線 (General Pipeline)
現代深度學習偵測器普遍採用模組化架構：
1. **Backbone（特徵提取骨幹）**：
   - 通常採用卷積神經網路（CNN）或 Vision Transformer，負責從原始像素網格中提取由低階到高階的多尺度特徵圖（Feature Maps）。
2. **Neck（特徵融合層）**：
   - 位於 Backbone 與 Head 之間（如 Feature Pyramid Network, FPN），負責跨尺度融合淺層的空間幾何細節與深層的高階語義特徵，以解決不同尺寸物體的偵測難題。
3. **Head（預測檢測頭）**：
   - 負責輸出最終的預測結果，包含邊界框座標迴歸（Bounding Box Regression: x, y, w, h）與類別機率分布（Class Probabilities）。

### 5. 兩大核心模型架構流派 (Two Main Model Paradigms)
- **Two-stage Detectors（兩階段偵測器，如 R-CNN 家族）**：
  - 第 1 階段：透過候選區域網路（RPN）或傳統演算法提出可能含有物體的候選框（Region Proposals）。
  - 第 2 階段：對所有候選區域單獨抽取特徵，進行精細分類與框座標修正。
  - 特點：定位精度高，但計算重複、速度相對較慢。
- **One-stage Detectors（單階段偵測器，如 YOLO, SSD）**：
  - 不經過獨立的候選框生成階段，直接將整張影像劃分為網格，單次前向傳播（Single Forward Pass）同時回歸出邊界框與類別。
  - 特點：推論速度極快（適合即時 Real-Time 任務），但早期版本在密集排列或極小物件上精度略遜。

### 6. 真實世界應用場景 (Real-World Applications)
- **自動駕駛（Autonomous Vehicles）**：即時識別行人、鄰近車輛、車道線、障礙物與交通號誌。
- **智慧醫療影像（Medical Imaging）**：在 X 光、CT、MRI 掃描中輔助醫生定位腫瘤、病變或組織結構。
- **智慧安防與監控（Security & Surveillance）**：周界防護、人群流量統計、異常行為預警。
- **工業製造與品質檢驗（Manufacturing & Robotics）**：機械手臂取件定位、產線零件缺陷檢測。
