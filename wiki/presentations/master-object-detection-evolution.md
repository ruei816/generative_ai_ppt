---
title: "物件偵測技術演進：20 頁教學簡報大綱"
type: "presentation"
created: "2026-11-18"
updated: "2026-11-18"
tags: [ObjectDetection, Presentation, Graph, R-CNN, YOLO, mAP, OBB]
sources:
  - "../../raw/articles/01-ibm-what-is-object-detection.md"
  - "../../raw/articles/02-lilian-weng-rcnn-family.md"
  - "../../raw/papers/03-yolov1-cvpr2016.md"
  - "../../raw/articles/04-roboflow-map-guide.md"
  - "../../raw/articles/05-ultralytics-obb-guide.md"
---

# 物件偵測技術演進：20 頁教學簡報

## 敘事主線
任務定義 → 通用架構 → Two-stage 演進 → One-stage 突破 → 評估指標 → 旋轉框應用。

## 20 頁頁序
1. 封面：讓電腦學會看世界
2. 人類看見物體，電腦讀取像素
3. 分類、定位、偵測與分割
4. 物件偵測的兩個核心：分類與定位
5. 通用偵測架構：Backbone、Neck、Head
6. Two-stage 與 One-stage 兩種解題思路
7. R-CNN：候選區域與重複特徵抽取
8. Fast R-CNN：共享特徵圖與 RoI Pooling
9. Faster R-CNN：RPN 自動產生候選框
10. Mask R-CNN：RoIAlign 與 Mask Branch
11. 為什麼追求 One-stage
12. YOLOv1：7×7 網格責任制
13. YOLOv1 的 7×7×30 輸出
14. YOLOv1 的速度優勢與定位限制
15. IoU：預測框與真實框的重疊程度
16. TP、FP、FN 與 NMS
17. Precision、Recall、AP、mAP
18. HBB 的限制與 OBB 的需求
19. OBB、Rotated IoU 與 Rotated NMS
20. 五篇來源的 Graph 知識全景與總結

## Graph 整合原則
- 簡報第 6–14 頁沿 Two-stage / One-stage 模型演進支線展開。
- 第 15–17 頁以 IoU → TP/FP/FN → Precision/Recall → AP → mAP 串成評估鏈。
- 第 18–19 頁以 HBB → OBB → Rotated IoU → Rotated NMS 串成幾何應用鏈。
- 每頁頁尾標示支持內容的來源；跨論文數據不作未控制條件的直接排名。

## 來源
- IBM Think：任務定義與通用偵測管線。
- Lilian Weng：R-CNN、Fast R-CNN、Faster R-CNN、Mask R-CNN。
- Redmon et al.：YOLOv1 原始論文。
- Roboflow：IoU、Precision、Recall、AP 與 mAP。
- Ultralytics：OBB 表示與旋轉框後處理。
