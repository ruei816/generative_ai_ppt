# Graph Schema 規範

## 1. Node（節點）

每個節點至少包含：

| 欄位 | 用途 |
|---|---|
| `id` | 全域唯一、穩定的英文 ID |
| `label` | 圖上顯示的繁體中文名稱，可保留英文術語 |
| `type` | `task`、`model`、`module`、`metric`、`application`、`concept`、`source` 之一 |
| `wiki` | 對應 Wiki 頁面的相對路徑；無對應頁面可省略 |
| `sources` | 支持此節點描述的 `raw/` 相對路徑 |
| `summary` | 一句話說明節點意義 |

## 2. Edge（關係）

每條關係至少包含：

| 欄位 | 用途 |
|---|---|
| `from` / `to` | 起點與終點節點 ID |
| `relation` | 關係標籤，如 `evolves_to`、`uses`、`evaluated_by` |
| `label` | 圖上顯示的繁體中文動詞或短語 |
| `sources` | 支持「這兩個節點存在此關係」的來源路徑 |

## 3. 關係詞彙建議

- `is_a`：屬於／是一種
- `contains`：包含／由…組成
- `uses`：使用
- `solves`：解決…問題
- `evolves_to`：演進為
- `compared_with`：與…比較
- `evaluated_by`：以…評估
- `applied_to`：應用於
- `requires`：需要／依賴
- `supports`：支援
- `used_to_calculate`：用於計算
- `summarized_by`：由…彙整
- `averaged_into`：平均後形成
- `represented_by`：以…表示
- `used_by`：被…使用

避免使用沒有明確語意的 `related_to`；若不得不用，必須在 `label` 或 `evidence_note` 解釋關係。

## 4. 證據與品質檢查

- 節點 `sources` 支持節點本身的定義；Edge `sources` 必須支持兩者之間的關係。
- Graph 中的數字、效能與比較結論仍以 Wiki / 原始來源為準，Graph 不得成為新事實的唯一來源。
- 每次更新執行：ID 唯一性、from/to 存在性、相對路徑有效性、孤立節點、無來源邊檢查。
- 視覺化前確認閱讀方向（例如 LR / TB）、主線與分支，避免交叉線遮蔽語意。
