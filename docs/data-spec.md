# 数据规范（草案 v0.1）

> 数据结构字段与 `src/guji_collate/schemas.py` 一一对应。改字段先改本文，再改代码。

## 1. 目录约定

```
data/
├── raw_images/     # 原始书页影像（默认 gitignore，仅保留 .gitkeep）
├── raw_texts/      # 原始电子文本（默认 gitignore）
├── normalized/     # 规整后文本（默认 gitignore）
├── output/         # 校勘结果、整理稿、校勘记（默认 gitignore）
└── editions/       # 底本与参校本目录（待建）
```

## 2. 文本行 JSON（OCR 输出，JSONL）

每行一条记录：

```json
{"page_no": 1, "text": "學而時習之", "confidence": 0.98, "x": 0.1, "y": 0.2, "w": 0.8, "h": 0.05}
```

- `x / y / w / h` 为整页比例坐标（0~1），便于不同分辨率下回显定位

## 3. 校勘记 JSON（一条异文）

```json
{
  "location": "卷一·页三·行四",
  "base_text": "底本文字",
  "variant_text": "参校本文字",
  "edition": "版本名",
  "note": "按语",
  "status": "pending"
}
```

- `status`：`pending` / `accepted` / `rejected`，人工复核后流转

## 4. 文本符号约定（待专家确认）

| 符号 | 含义 | 备注 |
| --- | --- | --- |
| `□` | 阙文 | |
| `■` | 漫漶 / 不可辨 | 待定 |
| `[ ]` | 补字 | |
| `( )` | 衍文 | 暂定 |
| `< >` | 错字纠正，后附按语 | 暂定 |

标点采用现代标点：`，。；：？！“”‘’《》`。

## 5. 元数据（每部书一份 `metadata.json`，待补充）

```json
{
  "title": "",
  "author": "",
  "dynasty": "",
  "base_edition": "",
  "compare_editions": [],
  "source": ""
}
```
