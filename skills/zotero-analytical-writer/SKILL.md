---
name: zotero-analytical-writer
description: "使用 Zotero metadata、批注和可访问的全文证据，按当前 Vault 模板创建或更新中文精读笔记，并确保 Zotero 链接、公式与原文引用真实可追溯。"
---

# Zotero Analytical Writer

## Canonical template

Resolve `vault_root` from the user-specified Vault or the current agent workspace that contains the target Note. For every Note creation or content update, first read the current Vault's own 精读模板 when it exists. Otherwise use the bundled fallback at `../../templates/论文精读模板.md`. The selected template is the structural authority for this task. Start a new Note from it; update an existing Note in place rather than introducing a parallel structure. All output paths must be real paths relative to `vault_root`, never machine-specific examples.

Keep the visible section order defined by the selected template. The bundled template uses:

`基本信息 → 一句话摘要 → 研究对象 → 研究方法（方法概述 → 方法分析） → 数据来源 → 研究结论 → 我的判断`

Keep the template's table, field labels, heading levels, and frontmatter fields. Replace every retained placeholder with verified content; remove unused optional formula blocks, surplus finding/quote pairs, and placeholder related-note links. Do not add main sections absent from the selected template unless the user requests them. Record limitations in `方法局限`、`数据局限` and `我的判断`; keep necessary scope and conditions beside the claims they qualify.

## 阅读深度

默认使用简版模板。
只有用户明确要求时，才增加综述或研究型论文的详细分析模块。
根据论文实际内容填写，不强制增加文献类型字段。

## 受控中文与术语说明

撰写或修改精读记录前，读取并使用 `ste-zh`。
从当前技能目录定位该技能，不硬编码个人路径。
若技能缺失，提示用户检查安装状态。
继续按以下原则完成可执行的写作。
不要声称已经调用缺失的技能。

- 按 ASD-STE100 中文化原则撰写。
- 解释性正文使用“中文-顺”档。
- 操作步骤使用 Strict 档。
- 每句只表达一件事，优先使用主动句。
- 同一概念使用同一名称，删除套话。
- 指令句以 25 字为目标，说明句以 40 字为目标。
- 保留原文条件、否定、数值和不确定性。
- 必要的精度优先于句长，保留长句时说明原因。
- 这些原则不代表官方中文认证。

所有专业术语须在正文首次出现处解释。
所有专业缩写须在首次出现处给出全称和中文名称。
定义须覆盖概念原理、本文作用和通俗解释。
指标还须说明计算口径、单位和数值方向。
术语表不能替代正文首次出现处的说明。
原文未定义时，可补充有依据的“概念说明”。
通用定义不得冒充论文采用的操作定义。

原文确实未报告时，写“原文未报告”。
当前材料不足时，写“当前材料不足，无法确认”。
不要按常见做法补齐参数、步骤或数值。
比较条件不同的数值分别记录，不直接排名。
区分绝对差、相对变化和百分点。
笔记作者自行计算时，标明公式和输入值。

## 职责与语料顺序

Writer 只负责分析层：`metadata + annotations + MinerU Fulltext + 必要时 PDF → 中文 analytical note`。

- Fulltext 是原文证据，分析笔记是中文理解层；不得把二者混为一谈。
- 仍保留现有的 `theme`、`study_area`、`data_source`、`methodology`、`core_variable`、`key_finding`、`relevance`、中文逻辑重构、学术垃圾过滤、公式防乱码、Zotero URI 和 Dataview 刷新逻辑。
- 不负责 PDF→MinerU；缺全文时交给 `zotero-fulltext-archiver`，不要自行伪造原文。
- 若已有正式 Fulltext，先复用并校验，不因 Note 模板修整而重新运行 MinerU。

## Knowledge handoff contract

The Analytical Note is the structural-library layer for later Knowledge synthesis; it is not a substitute for the Knowledge Page template. Preserve machine-readable fields and make the following handoff explicit whenever the Note will feed Knowledge:

- distinguish author-reported findings, methods, limitations, and the writer's interpretation;
- retain the exact `zotero_key`, `note_path`, `fulltext_path`, formulas, quotations, and PDF/Fulltext entry points;
- record which conclusions were checked against Fulltext and which remain Note-only;
- never mark a Knowledge claim as `fulltext_verified` merely because the Note contains a summary;
- if a field or conclusion cannot be located in the Note or Fulltext, write “未在当前材料中定位到可核验原文”，not a reconstructed quote or inferred page number.

When a later Knowledge task asks for “all papers”, the Knowledge Maintainer must read the Note's structured fields and the linked Fulltext independently; the Writer must not collapse the two layers into a single unsupported summary.

## Frontmatter 与稳定身份

Keep the template frontmatter (`title`、`aliases`、`tags`、`created`、`source`、`author`、`year`、`theme`、`study_area`、`data_source`、`methodology`、`core_variable`、`key_finding`、`relevance`) and replace its placeholders with verified values. Preserve an existing Note's `created`, `status` and other valid metadata; update `updated` when that field already exists. Do not add `paper_type` unless the selected template or user requires it. Add the following identity and navigation fields after the template fields when the values are known:

```yaml
type: literature-note
zotero_key: "Q22PFLNV"
pdf_key: "4RMSR7ZR"
doi: "..."
collection: "创新经济地理"
note_path: "02vault/创新经济地理/论文标题.md"
fulltext_path: "03fulltext/创新经济地理/Q22PFLNV.md"
```

New Analytical Notes are written only to `02vault/<collection>/<paper>.md`; new Fulltext records are written only to `03fulltext/<collection>/<zotero_key>.md`. Historical `note/` and `论文库/` files may be read during migration, but are never destinations for new files. Historical `fulltext/` links may be resolved as a read fallback; normalize a path only when updating that same Note.

## 原文引用规则

- “主要发现 + 原文引用”中的原文必须真实出现在 MinerU Fulltext 或 Original PDF。
- 严禁根据中文摘要反向生成英文 quote；定位不到时写“未在当前全文中定位到可核验原句”。
- 引用要保留必要上下文，检查否定、条件、稳健性限定、假设和局限。
- PDF 页码只有在可靠映射或实际 PDF 验证后才写入 `zotero://open-pdf/...?...page=` 或写入“PDF 第 n 页”；否则保留 Fulltext 链接并写“PDF 页码未验证”。不能因为 Fulltext 的 `page_mapping: unknown` 就猜页码，也不能因为页码未知而阻止通过原 PDF 直接核验引文。
- 结论区每一条“主要发现”都要紧邻至少一条真实原文引用；先在 MinerU Fulltext 定位，再在 Original PDF 核对上下文、否定、条件、稳健性和页码。找不到原句时明确写“未在当前全文中定位到可核验原句”，不得反向生成 quote。

## 内容与格式约束

- `theme` 概括研究问题；`study_area`、`data_source`、`methodology`、`core_variable`、`key_finding`、`relevance` 必须来自真实材料，不复制大段摘要。
- 过滤作者地址、单位、邮编、邮箱、基金号和排版垃圾；数据与方法缺失时如实说明。
- 公式仅在原文可靠时保留；乱码或残片不强行解释，也不为占位符生成符号拆解。
- 写入前检查 YAML、美元符号配对、`zotero://select/...` 和 PDF/Fulltext 入口。
- 写入前检查标题层级、模板占位符是否清除、结论是否成对、公式是否闭合，以及 `fulltext_path` 与 Note↔Fulltext 双向入口是否存在。
- 写入前检查术语首次说明。
- 写入前检查量化结果的指标、单位和比较条件。
- 使用 ste-zh 检查中文，不为缩短句子删掉限定。
- 模板或既有笔记包含 `关联精读笔记` 时，只保留可确认的真实关联；不为简版模板新增该章节。

## 写入与索引

1. 先判定是新建还是更新指定 Note；两种情况都先读取当前 Vault 模板或仓库随附模板。更新时允许重排正文以匹配模板，但不得改变稳定身份或制造重复 Note。
2. 在基本信息区的链接行加入 `全文 Markdown：[[03fulltext/<collection>/<zotero_key>]]`（路径存在时），并保留可验证的 Zotero 入口。
3. 新建论文笔记时才刷新四个根 Dataview 索引；更新既有 Note 不触发全库批量重写，但若标题、路径或索引字段变化，定向刷新受影响索引。
