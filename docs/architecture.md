# 架构设计（草案 v0.1）

## 1. 总览

```
书页影像 → [OCR 识别] → [文本规整] → [断句] → [自动标点] → [异文校勘] → [文白翻译] → 整理稿 + 校勘记
                                        ↓
                                  [人工审校 / 标注]
```

## 2. 模块职责

| 模块 | 目录 | 输入 | 输出 |
| --- | --- | --- | --- |
| OCR 识别 | `src/guji_collate/modules/ocr.py` | `PageImage` | `NormalizedText` |
| 文本规整 | 并入 OCR 或独立 `normalize.py`（待定） | `TextLine[]` | `NormalizedText` |
| 自动断句 | `modules/segmentation.py` | `NormalizedText` | `SegmentedText` |
| 自动标点 | `modules/punctuation.py` | `SegmentedText` | `PunctuatedText` |
| 异文校勘 | `modules/collation.py` | `NormalizedText` + 版本配置 | `VariantRecord[]` |
| 文白翻译 | `modules/translation.py` | `NormalizedText` | `TranslationResult` |
| 流水线编排 | `pipeline.py` | 上述阶段 | `CollationReport` |
| 服务接口 | `api/` | HTTP 请求 | JSON |

## 3. 数据流与契约

所有阶段只通过 `schemas.py` 中的 Pydantic 模型交换数据。字段变更必须同步 `docs/data-spec.md`，并在 PR 中说明影响到的阶段。

## 4. 技术选型（草案）

- **语言**：Python ≥ 3.10
- **服务**：FastAPI（`api` 可选依赖）
- **配置**：YAML（`configs/default.yaml` + `configs/local.yaml` 本地覆盖）
- **OCR**：PaddleOCR（可选依赖）
- **LLM**：OpenAI 兼容接口，可指向本地 / 私有化模型，保证古籍数据不出域
- **存储**：文本与校勘记先落文件（JSONL），后续评估 SQLite / PostgreSQL

## 5. 扩展点

- **OCR 引擎**：实现 `OCREngine.run(PageImage) -> NormalizedText` 即可注册替换
- **LLM 引擎**：统一走 OpenAI 兼容协议，模型名与 base_url 可配置
- **流水线阶段**：在 `Pipeline.register(name, stage)` 注册新阶段即可插入处理链

## 6. 待决策事项

- [ ] 文本规整独立成模块，还是作为 OCR 后处理
- [ ] 校勘比对采用字符级 diff、行级对齐，还是 LLM 直接判异
- [ ] 审校界面的形态：Web 页面、标注工具插件，或仅 API
