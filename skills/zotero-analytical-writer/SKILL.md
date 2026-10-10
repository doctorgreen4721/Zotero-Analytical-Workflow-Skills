---
name: zotero-analytical-writer
description: "使用 Zotero metadata、批注和可访问的全文证据，按当前 Vault 模板创建或更新中文精读笔记，并确保 Zotero 链接、公式与原文引用真实可追溯。"
---

# Zotero Analytical Writer

## Canonical template

Resolve `vault_root` from the user-specified Vault or the current agent workspace that contains the target Note. For every Note creation or content update, first read the current Vault's own 精读模板 when it exists. Otherwise use the bundled fallback at `../../templates/论文精读模板.md`. The selected template is the structural authority for this task. Start a new Note from it; update an existing Note in place rather than introducing a parallel structure. All output paths must be real paths relative to `vault_root`, never machine-specific examples.

Keep the visible section order exactly as follows:

`基本信息 → 一句话摘要 → 研究对象 → 研究方法（方法概述 → 方法分析） → 数据来源 → 研究结论 → 关联精读笔记 → 我的判断`

Keep the template's table, field labels, heading levels, and frontmatter fields. Keep applicable type-specific subsections within the existing main sections. Replace every retained placeholder with verified content; remove unused type modules, optional formula blocks, surplus finding/quote pairs, and placeholder related-note links. Do not add legacy main headings such as `研究对象与问题`, `整体流程`, `数据与证据定位`, `局限与后续问题`, or `Zotero信息`. Record detailed limitations in `方法局限`、`数据局限` and `我的判断`; keep necessary scope and conditions beside the claims they qualify.

## 文献类型与阅读脉络

先根据全文确定文献类型。不要只看标题。
`paper_type` 使用“综述”“研究型”“混合”或“待确认”。
材料不足时保留“待确认”，并说明缺少的证据。

- 综述使用标有“综述”的模块。
- 研究型论文使用标有“研究型”的模块。
- 混合型论文保留两类必要模块。
- 理论论文不套用模型训练或实验流程。
- 当前 Vault 模板缺少类型模块时，在原主章节内补充。
- 补充模块参考随附模板，保留本地字段和标题层级。
- 同一事实只写一次，其他位置引用对应段落。

综述须解释难题、研究分类和解题机制。
记录代表学者、评价标准、共识与分歧。
记录各类方法的适用条件和具体缺点。
说明作者的综合判断、新见解和方法论贡献。
区分被综述学者、综述作者和笔记作者的观点。
转述结果标为“据本综述转述”。
没有读过对应原研究时，不声称直接验证。

研究型论文先从摘要和结论提取成果。
再回溯支撑成果的数据、方法和使用条件。
正文按研究操作顺序说明输入、操作和输出。
区分既有方法、方法组合和本文创新点。
分别说明训练条件与应用条件。
记录量化效果、基线和比较条件。
不将模板示例阶段当作论文实际步骤。

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

Keep the template frontmatter (`title`、`aliases`、`tags`、`created`、`source`、`author`、`year`、`theme`、`study_area`、`data_source`、`methodology`、`core_variable`、`key_finding`、`relevance`) and replace its placeholders with verified values. Also preserve `type`、`status`、`updated` and `paper_type` when the template includes them. Preserve an existing Note's `created` and `status`; update `updated` on a content change and add `paper_type` using the values above. Add the following identity and navigation fields after the template fields when the values are known:

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
- 写入前检查文献类型、阅读脉络和术语首次说明。
- 写入前检查量化结果的指标、单位和比较条件。
- 使用 ste-zh 检查中文，不为缩短句子删掉限定。
- `关联精读笔记` 只保留与研究问题、方法或结论存在直接关系的真实 Note；没有可确认的关联时，保留章节说明但移除示例链接。

## 写入与索引

1. 先判定是新建还是更新指定 Note；两种情况都先读取当前 Vault 模板或仓库随附模板。更新时允许重排正文以匹配模板，但不得改变稳定身份或制造重复 Note。
2. 在基本信息区的链接行加入 `全文 Markdown：[[03fulltext/<collection>/<zotero_key>]]`（路径存在时），并保留可验证的 Zotero 入口。
3. 新建论文笔记时才刷新四个根 Dataview 索引；更新既有 Note 不触发全库批量重写，但若标题、路径或索引字段变化，定向刷新受影响索引。
