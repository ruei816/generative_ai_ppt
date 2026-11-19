# Object Detection LLMwiki — 操作歷史日誌 (log.md)

本文件以時間順序記錄對此知識庫進行的所有重要操作（Ingest, Query, Lint, Refactor）。
所有日誌項目均以標準前綴 `## [YYYY-MM-DD] <action> | <title>` 開頭，方便命令列檢索。

---

## [2026-11-17] init | 知識庫架構初始化與 AGENTS.md 規範確立
- 確立 Karpathy LLMwiki 架構：Raw（不可變真理層）、Wiki（LLM 維護層）、Schema（`AGENTS.md` 協議）。
- 建立目錄骨架：`raw/`（papers, articles, assets）、`wiki/`（basics, models, concepts, benchmarks, applications, synthesis）。
- 確立採用標準 Markdown 相對路徑超連結規範，以及繁體中文 + 英文專業術語風格。
- 確立初期 5 篇核心文獻的錄入清單與模組對應表。

## [2026-11-17] schema-update | 納入四大核心準則與簡報產出規範
- **初學者友善原則**：頁面全面導入「痛點 -> 動機 -> 演進」的敘事框架與直觀圖解。
- **客觀事實與零額外推論**：嚴格規範論文矛盾處理方式，強制以「對照證明表」呈現雙方實驗條件與原文證據，嚴禁 Agent 主觀臆測。
- **全量資訊來源標註**：所有數據指標與觀點必須帶有精確出處。
- **簡報產出 SOP 確立**：建立 `wiki/presentations/` 目錄與 Marp 格式投影片生成流程。

## [2026-11-17] ingest | IBM Think: What is Object Detection?
- **來源存檔**：`raw/articles/01-ibm-what-is-object-detection.md`（作者：Jacob Murel, Ph.D. & Eda Kavlakoglu, IBM Research, 2024）。
- **建立實體頁面**：
  - `wiki/basics/task-definition.md`（任務四分法、What+Where 雙重任務直觀解析）
  - `wiki/basics/general-architecture.md`（數位像素網格、Backbone-Neck-Head 通用管線、兩大典範）
- **漣漪更新 (Ripple Updates)**：
  - 建立 `wiki/concepts/backbones/overview.md`
  - 建立 `wiki/concepts/necks/overview.md`
  - 建立 `wiki/concepts/heads/overview.md`
  - 建立 `wiki/synthesis/two-stage-vs-one-stage.md`
- **更新索引**：同步更新 `index.md`。

## [2026-11-17] ingest | Lil'Log: R-CNN Family (Lilian Weng)
- **來源存檔**：`raw/articles/02-lilian-weng-rcnn-family.md`（作者：Lilian Weng, 2017）。
- **建立實體頁面**：
  - `wiki/models/rcnn-family.md`（四代模型 R-CNN, Fast R-CNN, Faster R-CNN, Mask R-CNN 演進脈絡、PASCAL VOC 2007 數據）
- **漣漪更新 (Ripple Updates)**：
  - `wiki/concepts/assignment/rpn.md`（RPN 與 Anchor Boxes 機制、正負樣本匹配）
  - `wiki/concepts/heads/roi-pooling-and-align.md`（RoIPool vs RoIAlign、量化取整與雙線性插值）
  - `wiki/concepts/losses/smooth-l1.md`（Smooth L1 Loss 數學定義與魯棒性對比）
  - `wiki/concepts/postprocessing/nms.md`（NMS 流程、遮擋誤刪痛點與 NMS-free 演進）
  - `wiki/synthesis/two-stage-vs-one-stage.md`（補充 Two-stage 實驗數據與來源引用）
- **簡報產出**：
  - `wiki/presentations/02-rcnn-family-evolution.md`（Marp 投影片：Two-stage 演進史與 5 FPS 瓶頸）
- **更新索引**：同步更新 `index.md`。

## [2026-11-17] ingest | CVPR 2016: YOLOv1 (arXiv:1506.02640)
- **來源存檔**：`raw/papers/03-yolov1-cvpr2016.md`（作者：Joseph Redmon et al., CVPR 2016）。
- **建立實體頁面**：
  - `wiki/models/yolov1.md`（單一迴歸問題重塑、$S \times S$ 網格機制、$7 \times 7 \times 30$ 張量、Titan X 45 FPS 實測數據、Hoiem et al. 錯誤成分診斷）
- **漣漪更新 (Ripple Updates)**：
  - `wiki/concepts/heads/dense-heads.md`（密集預測頭機制與網格全域視野）
  - `wiki/concepts/losses/yolov1-multi-part-loss.md`（多部分加權均方和誤差、$\lambda_{\text{coord}}=5$、$\lambda_{\text{noobj}}=0.5$ 與開根號設計）
  - `wiki/synthesis/two-stage-vs-one-stage.md`（建立 YOLOv1 vs Fast/Faster R-CNN 實測數據與客觀錯誤對比表，記錄 75.0% 集成互補事實）
- **簡報產出**：
  - `wiki/presentations/03-yolov1-single-stage-breakthrough.md`（Marp 投影片：登高瞭望比喻、網格機制、45 FPS 即時性與 Fast R-CNN 互補診斷）
- **更新索引**：同步更新 `index.md`。

## [2026-11-17] ingest | Roboflow: What is Mean Average Precision (mAP)?
- **來源存檔**：`raw/articles/04-roboflow-map-guide.md`（來源：Roboflow Blog, 2023-2024）。
- **建立實體頁面**：
  - `wiki/benchmarks/iou.md`（IoU 計算幾何定義、三大管線職責、重複框 FP 懲罰機制）
  - `wiki/benchmarks/map.md`（mAP 五步計算流程、P-R 曲線 AUC、mAP@50 與 mAP@50:95 差異、COCO 尺度指標）
- **簡報產出**：
  - `wiki/presentations/04-map-and-iou-explained.md`（Marp 投影片：射箭計分比喻、貼紙重疊面積、Precision-Recall 天平、mAP@50 vs 50:95 殘酷落差）
- **更新索引**：同步更新 `index.md`。

## [2026-11-17] ingest | Ultralytics: Oriented Bounding Box (OBB) Detection
- **來源存檔**：`raw/articles/05-ultralytics-obb-guide.md`（來源：Ultralytics YOLO Docs, 2024）。
- **建立實體頁面**：
  - `wiki/applications/obb.md`（旋轉框原理、HBB 痛點、xywhr 與 xyxyxyxy 格式、角度週期對稱性邊界問題、DOTA v1.0 數據）
- **漣漪更新 (Ripple Updates)**：
  - `wiki/concepts/postprocessing/rotated-nms.md`（Rotated NMS 演算法、Sutherland-Hodgman 多邊形求交與真實重疊計算）
  - `wiki/synthesis/hbb-vs-obb.md`（水平框 vs 旋轉框橫向全方位對比、標註成本與落地選型決策樹）
  - `wiki/benchmarks/iou.md`（補充旋轉交併比 Rotated IoU 幾何擴展）
- **簡報產出**：
  - `wiki/presentations/05-oriented-bounding-boxes.md`（Marp 投影片：紙箱裝吉他比喻、背景雜訊與 NMS 誤殺痛點、角度跳躍、Rotated NMS 與 5 講知識總結）
- **更新索引**：同步更新 `index.md`，完成全系列 5 篇核心文獻的完整錄入！

## [2026-11-17] lint | 全庫健康檢查與連結校驗通過
- **檢查項目**：孤島頁面、相對路徑死鏈、YAML Frontmatter 格式對齊。
- **檢查結果**：全庫共 31 個 Markdown 檔案（6 個原始真理檔，25 個 Wiki 知識檔），所有跨層級 Markdown 相對路徑超連結 100% 驗證有效無死鏈。

## [2026-11-17] presentation | 產出旗艦大綱教學簡報 (master-object-detection-evolution.md)
- **產出檔案**：`wiki/presentations/master-object-detection-evolution.md`。
- **簡報特性**：
  - 專為一般大眾與初學者設計的 14 頁 Marp Markdown 投影片。
  - 整合五大文獻：任務定義（夜市比喻）、R-CNN 演進（影印店比喻）、YOLOv1 迴歸（瞭望守衛比喻）、IoU/mAP 計分（射箭天平）、旋轉框 OBB（紙箱裝吉他比喻）。
  - 包含客觀論文數據對照表（Titan X GPU 實測、背景誤檢 vs 定位誤差、PASCAL VOC 與 DOTA 指標），無主觀腦補。
  - 每頁底部均附帶完整來源追溯。
- **更新索引**：同步更新 `index.md`。

## [2026-11-17] presentation-html | 建立獨立互動式簡報網頁 (index.html)
- **產出檔案**：`index.html`（專案根目錄）與 `wiki/presentations/index.html`。
- **網頁特性**：
  - 獨立單一檔案（Self-contained），免安裝任何 Markdown 或 Marp 外掛，用任何瀏覽器（Chrome, Edge, Safari）雙擊即可直接全螢幕放映。
  - 支援鍵盤翻頁（左右鍵、空白鍵、PageUp/Down、Home/End）、全螢幕按鍵（F）、大綱目錄跳轉（O）。
  - 內建平滑卡片動畫、頂部進度條、精準數據表格與全量資訊來源註腳。
- **更新索引**：同步更新 `index.md`。

## [2026-11-17] theme-update | 導入使用者專屬配色（Ivory, Muted Teal, Dusty Olive, Chocolate Plum）
- **色盤設定**：
  - `#F5FBEF` (Ivory 象牙白)
  - `#92AD94` (Muted Teal 鼠尾草青綠)
  - `#748B75` (Dusty Olive 橄欖綠)
  - `#503D42` (Chocolate Plum 深焙梅紫)
- **實踐內容**：全面升級 `index.html` 與 `wiki/presentations/index.html`，支援「深焙梅紫沉浸夜間模式」與「象牙雅緻明亮模式」一鍵切換（鍵盤快捷鍵 `T`）。


## [2026-11-18] graph-integration | 導入 Graph 知識關聯層與製作 SOP
- **新增 Graph Layer**：建立 `graph/`，將節點與關係從一般 Wiki 文章中獨立管理。
- **建立製作規範**：新增 `graph/README.md` 與 `graph/schema.md`，納入需求範圍、資料盤點、Graph 規劃、JSON 建構、大綱確認、視覺化、簡報整合與初版檢查。
- **建立物件偵測 Graph**：新增 `object-detection-graph.json` 與 `object-detection-graph.mmd`，涵蓋任務定義、Backbone/Neck/Head、Two-stage/R-CNN/RPN、One-stage/YOLOv1、IoU/mAP、HBB/OBB 與 Rotated NMS。
- **更新維護協議**：更新 `AGENTS.md`、`index.md` 與 `wiki/README.md`，規定 Wiki 變更後同步檢查 Graph，且每條關係需有來源證據。


## 2026-11-18｜20 頁物件偵測教學簡報製作
- 依使用者核准的 20 頁內容重製專案根目錄 `index.html` 與 `wiki/presentations/index.html`，沿用原色彩與互動模板。
- 新增 Two-stage / One-stage、R-CNN 家族、YOLOv1、IoU/mAP、OBB 等示意圖與跨頁知識主線。
- 擴充 `graph/object-detection-graph.json`，同步產生 Mermaid 總覽。
- 增加響應式版面、觸控切頁與 20 頁目錄導覽；完成 HTML 結構、JavaScript 語法、圖片路徑及 Graph 節點／關係來源路徑檢查。
