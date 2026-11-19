# wiki/ — LLM 維護之知識庫主體 (Compounding Artifact)

本目錄存放所有由 AI Agent 自動生成、交叉引用與動態維護的知識頁面。Graph 關係資料另存於專案根目錄 `graph/`，以避免把知識敘述與關係索引混在同一層。
所有頁面均採用標準 Markdown 相對路徑相互連結，並遵循 YAML Frontmatter 格式。

---

## 子目錄分類

- `basics/`：電腦視覺基礎任務定義與通用架構管線。
- `models/`：各架構模型專頁（R-CNN 家族、YOLO 系列、DETR 等）。
- `concepts/`：核心模組深度解析：
  - `backbones/`：特徵提取網絡。
  - `necks/`：多尺度特徵融合。
  - `heads/`：檢測頭與預測輸出。
  - `losses/`：定位與分類損失函數。
  - `assignment/`：正負樣本分配機制。
  - `postprocessing/`：NMS 與後處理演算法。
- `benchmarks/`：評估指標（IoU, mAP, FPS）與資料集（COCO 等）。
- `applications/`：特殊場景應用（OBB 旋轉框、遙感影像等）。
- `synthesis/`：跨模型橫向對比、技術演進脈絡總結。


## Graph 關聯層

- Graph 規劃與維護請參考 [`../graph/README.md`](../graph/README.md)。
- 新增或修改知識頁面後，需同步檢查 Graph JSON 中對應節點、關係與來源路徑。
