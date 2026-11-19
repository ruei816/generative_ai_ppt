# Object Detection LLMwiki — 總目錄索引 (index.md)

本索引是知識庫的內容導航總表。Agent 執行 Ingest 或 Query 時，應先閱讀本文件以快速掌握既有內容與關聯路徑。

---

## 1. 基本任務與通用架構 (Basics)
*涵蓋電腦視覺基礎任務分類、影像網格表示法以及現代物件偵測的通用模組管線。*

- [電腦視覺基礎任務定義：影像分類、物件定位與物件偵測](wiki/basics/task-definition.md) — 釐清 Classification, Localization, Detection 與 Segmentation 的本質差異，分析 What + Where 雙重任務。
- [現代物件偵測通用架構：從像素網格到 Backbone、Neck 與 Head](wiki/basics/general-architecture.md) — 剖析數位影像取樣量化，解構現代偵測器的三層通用管線與兩大架構流派。

---

## 2. 檢測模型專頁 (Models)
*涵蓋 Two-stage、One-stage、Anchor-free、DETR/Transformer 與開放詞彙等代表性模型。*

### Two-stage 模型家族
- [R-CNN 家族演進史：從 R-CNN、Fast R-CNN、Faster R-CNN 到 Mask R-CNN](wiki/models/rcnn-family.md) — 梳理候選框生成、特徵共享、RPN 與 RoIAlign 的四代演進脈絡與速度實驗數據對照。

### One-stage 模型家族
- [YOLOv1：You Only Look Once 單階段即時物件偵測開山之作](wiki/models/yolov1.md) — 將檢測視為單一迴歸問題，$S \times S$ 網格劃分、45 FPS 即時推論與錯誤成分診斷。

---

## 3. 核心技術與模組 (Concepts)

### 3.1 特徵提取骨幹 (Backbones)
- [特徵提取骨幹網絡總覽 (Backbones Overview)](wiki/concepts/backbones/overview.md) — 從傳統手工特徵到 CNN/Transformer 多尺度特徵提取的演進。

### 3.2 特徵融合層 (Necks / FPN)
- [特徵融合層總覽 (Necks Overview)](wiki/concepts/necks/overview.md) — 解決多尺度物體偵測難題的關鍵樞紐，從 FPN 到 PANet 的演進。

### 3.3 檢測頭與預測機制 (Heads)
- [檢測頭與預測機制總覽 (Heads Overview)](wiki/concepts/heads/overview.md) — 負責座標迴歸與類別機率預測的終端組件，Sparse vs Dense vs Transformer Heads。
- [候選區域特徵採樣：RoI Pooling 與 RoIAlign 機制剖析](wiki/concepts/heads/roi-pooling-and-align.md) — 比較量化取整與雙線性插值的空間對齊精度。
- [密集預測頭 (Dense Prediction Heads) 機制與網格回歸](wiki/concepts/heads/dense-heads.md) — 全圖網格一次性出框，高硬體親和度與全域視野。

### 3.4 損失函數 (Loss Functions)
- [平滑 L1 損失函數 (Smooth L1 Loss) 機制與數學分析](wiki/concepts/losses/smooth-l1.md) — 剖析結合 L1 魯棒性與 L2 平滑性的邊界框回歸損失。
- [YOLOv1 多部分加權均方誤差損失函數 (Multi-Part SSE Loss) 詳解](wiki/concepts/losses/yolov1-multi-part-loss.md) — 拆解五大子項、$\lambda_{\text{coord}}=5$ 與 $\lambda_{\text{noobj}}=0.5$ 加權天平與開根號設計。

### 3.5 樣本匹配機制 (Label Assignment)
- [區域候選網絡 (RPN) 與錨框 (Anchor Boxes) 機制](wiki/concepts/assignment/rpn.md) — 解析神經網絡如何基於 IoU 門檻在多尺度錨框上進行正負樣本分配。

### 3.6 後處理與篩選機制 (Postprocessing)
- [非極大值抑制 (Non-Maximum Suppression, NMS) 演算法詳解](wiki/concepts/postprocessing/nms.md) — 剖析消除重疊框的經典演算法、密集遮擋誤刪痛點與演進方向。
- [旋轉非極大值抑制 (Rotated NMS) 與旋轉交併比 (Rotated IoU)](wiki/concepts/postprocessing/rotated-nms.md) — 利用多邊形求交裁剪演算法計算真實旋轉重疊面積，解決密集斜向目標誤刪。

---

## 4. 評估指標與基準資料集 (Benchmarks)
*涵蓋 IoU、mAP、P-R 曲線、推論延遲與標準資料集。*

- [交併比 (Intersection over Union, IoU) 計算原理與評估機制](wiki/benchmarks/iou.md) — 衡量邊界框重疊度的幾何指標，在正負樣本匹配、NMS 與 TP/FP 判定中的核心地位與重複框懲罰規則。
- [平均精度均值 (Mean Average Precision, mAP) 計算指南與 COCO 評估標準](wiki/benchmarks/map.md) — 拆解 Precision-Recall 曲線、PASCAL VOC 11 點插值與 COCO mAP@50:95 黃金標準。

---

## 5. 進階應用場景 (Applications)
*涵蓋特殊檢測任務，如旋轉框（OBB）、遙感航拍、傾斜文字、紅外與小物件檢測。*

- [旋轉邊界框 (Oriented Bounding Box, OBB) 物件偵測原理與應用](wiki/applications/obb.md) — 航拍遙感場景、xywhr 與 xyxyxyxy 格式、角度週期對稱性難題與 DOTA Benchmark。

---

## 6. 橫向對比與綜合專題 (Synthesis)
*跨模型演進比較、工程落地選型指南與論文矛盾事實證明表。*

- [兩階段 (Two-stage) 與單階段 (One-stage) 檢測器架構對比與演進史](wiki/synthesis/two-stage-vs-one-stage.md) — 客觀並列兩大派別在候選框、推論延遲 (FPS) 與小目標精度上的設計取捨與實測數據。
- [水平框 (HBB) 與旋轉框 (OBB) 全方位技術對比與選型指南](wiki/synthesis/hbb-vs-obb.md) — 全方位對比標註成本、幾何計算延遲、密集排列召回率與工程落地決策樹。

---

## 7. Graph 知識關聯圖 (Graph Layer)
*以結構化節點與關係呈現物件偵測知識脈絡，作為 Wiki 導覽與簡報敘事規劃依據。*

- [Graph 製作與維護規範](graph/README.md) — 需求確認、資料盤點、Graph 規劃、結構化建構、視覺化與檢查流程。
- [Graph Schema](graph/schema.md) — Node、Edge、關係詞彙與來源證據欄位規範。
- [物件偵測知識關聯圖（JSON）](graph/object-detection-graph.json) — 可維護、可檢查的結構化節點與關係資料。
- [物件偵測知識關聯圖（Mermaid）](graph/object-detection-graph.mmd) — 可直接嵌入 Markdown 的視覺化版本。

---

## 8. 教學簡報與投影片 (Presentations)

*適合演講、教學與分享的投影片，強調初學者演進視角與客觀來源。*

- 🌐 **[互動式簡報網頁（專案根目錄 index.html）](index.html)** — 20 頁版本，包含全螢幕放映、大綱跳轉、進度條、鍵盤與觸控切頁、明暗主題切換。另有 `wiki/presentations/index.html` 同步副本。
- 🌟 **[旗艦大綱投影片：讓電腦學會「看」世界 —— 物件偵測十年演進史 (全五講合一)](wiki/presentations/master-object-detection-evolution.md)** — 面向初學者的 20 頁教學簡報大綱，依 Graph 主線串聯五篇來源。
- [第 01 講：什麼是物件偵測？從像素網格到現代通用檢測架構](wiki/presentations/01-intro-to-object-detection.md) — 初學者友善投影片（夜市生活化比喻、任務四分法、通用管線與兩大流派取捨）。
- [第 02 講：Two-stage 偵測器演進史：從 2000 個候選框到 RPN 端到端學習](wiki/presentations/02-rcnn-family-evolution.md) — 辦公室影印店比喻、四代架構逐層拆解、PASCAL VOC 2007 實測耗時與 5 FPS 瓶頸討論。
- [第 03 講：One-stage 核心突破：YOLOv1 單一回歸如何跑出 45 FPS 極速](wiki/presentations/03-yolov1-single-stage-breakthrough.md) — 登高瞭望比喻、網格責任指派、$7 \times 7 \times 30$ 輸出張量、加權損失與 Fast R-CNN 互補診斷分析。
- [第 04 講：怎麼客觀評價「框得準不準」？IoU 與 mAP 計算完全解密](wiki/presentations/04-map-and-iou-explained.md) — 射箭計分比喻、貼紙交疊面積、P-R 曲線面積與 mAP@50 vs mAP@50:95 殘酷落差解析。
- [第 05 講：進階應用 —— 為什麼斜著的物體框不住？旋轉框 OBB 登場！](wiki/presentations/05-oriented-bounding-boxes.md) — 紙箱裝吉他比喻、背景雜訊與 NMS 誤殺痛點、角度週期跳躍難題、Rotated NMS 與選型決策樹。
