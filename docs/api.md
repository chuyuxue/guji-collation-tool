# 接口设计（草案 v0.1）

Base URL：`/api/v1`，JSON 请求 / 响应。完整契约待功能需求落地后细化。

| 方法 | 路径 | 说明 | 状态 |
| --- | --- | --- | --- |
| GET | `/health` | 健康检查 | 已占位 |
| POST | `/ocr` | 上传影像（multipart），返回文本行 | 待开发 |
| POST | `/segment` | 文本断句 | 待开发 |
| POST | `/punctuate` | 自动标点 | 待开发 |
| POST | `/collate` | 底本 + 参校本文本，返回校勘记列表 | 待开发 |
| POST | `/translate` | 文白翻译 | 待开发 |

## 请求 / 响应示例（草案）

### POST /punctuate

请求：

```json
{"text": "學而時習之不亦說乎"}
```

响应：

```json
{
  "text": "學而時習之，不亦說乎。",
  "segments": [
    ["學而時習之", "學而時習之，"],
    ["不亦說乎", "不亦說乎。"]
  ]
}
```

### POST /collate

请求：

```json
{
  "base_edition": "底本",
  "compare_editions": ["参校本A"],
  "base_text": "……",
  "compare_texts": {"参校本A": "……"}
}
```

响应：

```json
{
  "variants": [
    {
      "location": "卷一·页三·行四",
      "base_text": "底本文字",
      "variant_text": "参校本文字",
      "edition": "参校本A",
      "note": "",
      "status": "pending"
    }
  ]
}
```
