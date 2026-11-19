# raw/ — 原始資料存放庫 (Source of Truth)

本目錄存放所有**原始、不可變（Immutable）**的參考資料。
AI Agent 僅能讀取此目錄下的檔案以提取事實，**嚴禁修改或刪除此目錄內的內容**。

---

## 目錄結構說明

- `papers/`：學術論文原文（PDF 或轉換後的 Markdown 原文）。
- `articles/`：技術部落格、官方文件、教學文章等網頁剪貼。
- `assets/`：原始圖檔（架構圖、流程圖、P-R 曲線圖等）。

---

## 預計收錄的第 1~5 篇資料清單

1. **`articles/01-ibm-what-is-object-detection.md`**
   - 標題：What is Object Detection?
   - 來源：IBM Research (Jacob Murel Ph.D. & Eda Kavlakoglu)
   - 重點：基本任務定義（Classification vs. Localization vs. Detection）、通用 Pipeline（Backbone, Neck, Head）。

2. **`articles/02-lilian-weng-rcnn-family.md`**
   - 標題：Object Detection for Dummies Part 3: R-CNN Family
   - 來源：Lil'Log (Lilian Weng)
   - 重點：Two-stage 演進（R-CNN Selective Search -> Fast R-CNN RoI Pooling -> Faster R-CNN RPN）、NMS 機制。

3. **`papers/03-yolov1-cvpr2016.md`**
   - 標題：You Only Look Once: Unified, Real-Time Object Detection
   - 來源：CVPR 2016 (Joseph Redmon et al., arXiv:1506.02640)
   - 重點：One-stage 迴歸機制、S×S 網格、即時推論與小物件 Trade-off。

4. **`articles/04-roboflow-map-guide.md`**
   - 標題：What is Mean Average Precision (mAP) in Object Detection?
   - 來源：Roboflow
   - 重點：IoU 定義、Precision-Recall 曲線、mAP@50 與 mAP@50:95 計算。

5. **`articles/05-ultralytics-obb-guide.md`**
   - 標題：Oriented Bounding Box (OBB) Detection
   - 來源：Ultralytics YOLO Docs
   - 重點：旋轉框應用（遙感、傾斜文字）、座標與角度表示法、角度週期邊界問題。
