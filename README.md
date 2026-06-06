# 🀄 Chinese Character Content

> Open, ready-to-use **Chinese character learning datasets** for English speakers — frequency-ranked, pinyin-annotated, HSK-inspired, and CSV/JSON-clean.

**English | [中文](#中文)**

[![Last commit](https://img.shields.io/github/last-commit/shengdabai/Chinese-character-content)](https://github.com/shengdabai/Chinese-character-content/commits)
[![Stars](https://img.shields.io/github/stars/shengdabai/Chinese-character-content?style=social)](https://github.com/shengdabai/Chinese-character-content/stargazers)
[![Follow @shengdabai](https://img.shields.io/github/followers/shengdabai?style=social)](https://github.com/shengdabai)
[![License: CC BY-SA 4.0](https://img.shields.io/badge/License-CC%20BY--SA%204.0-blue.svg)](LICENSE.md)

---

## Why this exists

Most Chinese-character word lists are locked inside PDFs, spreadsheets, or apps you can't query. Beginners and HSK 1–3 learners need **structured, machine-readable data** — frequency rank, pinyin, English meaning, example words, and learner-friendly bands — that they can drop straight into Anki, a spreadsheet, a web app, or an LLM prompt.

This repo merges three authoritative character sources into one clean, well-documented dataset, then ships speaking-first lesson content and printable study materials on top of it. Everything is **static, open, and reproducible** — no login, no database, no API key.

Built in public by a Chinese-language teacher (6,000+ students) building AI + Chinese-teaching tools.

## What it is

- A **merged character database** of 15,001 characters, each tagged with frequency rank, corpus coverage, pinyin, English definitions, and source provenance.
- A **top-600 learner subset** grouped into HSK-inspired bands (Zero Beginner → HSK3 Expansion).
- A **3,500 common-character** quick-reference and category-grouped lookup.
- **Speaking-first lessons**, drill cards, a 4-week plan, a business phrasebook, an Anki deck, and printable PDFs — all generated from the same data.

> The level system is **HSK-inspired**, not an official HSK syllabus copy. Vocabulary is **speaking-first**; writing drills and stroke order are intentionally excluded.

## ✨ What's inside

```
data/
  raw/            3 source spreadsheets (3500 list, 7000 list, 2.5B-word corpus)
  processed/      10 clean CSVs (the datasets below) + build_summary.json
content/          lessons + product packs + companion materials (JSON)
pdf/              5 printable study packs
docs/             static GitHub Pages site
scripts/          build_materials.py (regenerates everything)
```

### Datasets (`data/processed/`)

| File | Rows | What it gives you |
|---|---:|---|
| `characters_master.csv` | 15,001 | Full merged DB — 3500 + 7000 + corpus, with frequency, pinyin, definitions, provenance |
| `common_3500_quick_reference.csv` | 3,500 | Pinyin-first lookup sheet for the 3,500 most common characters |
| `common_3500_by_category.csv` | 3,500 | The 3,500 grouped into learner-friendly categories |
| `learning_characters.csv` | 600 | Top-600 learning set, grouped by HSK-inspired band |
| `anki_top_600.csv` | 600 | Anki-ready `front,back,tags` deck for the top 600 |
| `lesson_vocabulary.csv` | 124 | Vocabulary across 12 speaking-first lessons |
| `speaking_drill_cards.csv` | 15 | Prompt → Chinese → pinyin → English drill cards |
| `business_phrasebook.csv` | 12 | Business-scenario phrases with pinyin + English |
| `placement_checklist.csv` | 12 | Can-do self-placement checklist |
| `four_week_plan.csv` | 4 | Themed 4-week speaking study plan |

### Content (`content/`)

| File | Shape | Contents |
|---|---|---|
| `lessons.json` | array | 12 lessons (`id`, `level`, `title`, `goal`, `terms[]`) |
| `product_packs.json` | array | Productized learning-pack ideas + positioning |
| `learning_materials_companion.json` | object | `four_week_plan`, `speaking_drills`, `business_phrases`, `placement_checklist` |

## 🧱 Format / Tech

- **Data:** UTF-8 CSV + JSON. No proprietary formats in the outputs.
- **Build:** Python 3 (`pandas`, `openpyxl`, `xlrd`, `reportlab`, `pypinyin`).
- **Definitions:** enriched from **CC-CEDICT** when the build can download it; pinyin falls back to `pypinyin`.
- **Site:** plain static HTML in `docs/` (GitHub Pages ready, `.nojekyll`).

## 🚀 How to use the data

Use the CSVs directly — no build required.

**Python / pandas**
```python
import pandas as pd
df = pd.read_csv("data/processed/characters_master.csv")
top100 = df[df["frequency_tier"] == "Top 100"][["character", "pinyin", "definition_en"]]
print(top100.head())
```

**Anki** — import `data/processed/anki_top_600.csv` (fields: `front`, `back`, `tags`; the `back` field contains HTML).

**JavaScript**
```js
const lessons = await fetch("content/lessons.json").then(r => r.json());
console.log(lessons[0].title, lessons[0].terms.length);
```

**Rebuild from source** (optional)
```bash
python3 -m venv .venv
.venv/bin/python -m pip install -r requirements.txt
.venv/bin/python scripts/build_materials.py
```

**Preview the site**
```bash
python3 -m http.server 8080 -d docs
```

## 📖 Data dictionary — `characters_master.csv`

| Column | Meaning |
|---|---|
| `character` | The Chinese character |
| `pinyin` / `pinyin_plain` / `pinyin_initial` | Toned pinyin / toneless / first letter |
| `definition_en` | English definition (CC-CEDICT-derived) |
| `example_words` | Common words using the character, with pinyin |
| `lookup_category` | Learner-friendly category (e.g. *Actions*, *Question & Function*) |
| `frequency_tier` | Top 100 / Top 300 / … band |
| `learning_band` | HSK-inspired band (Zero Beginner → HSK3 Expansion) |
| `frequency_rank` | Corpus frequency rank |
| `token` | Raw corpus token count |
| `frequency_per_million` | Frequency normalized per million tokens |
| `coverage_rate_pct` | Cumulative text-coverage percentage |
| `rank_3500` / `rank_7000` | Rank within the 3500 / 7000 source lists |
| `in_3500` / `in_7000` / `in_corpus` | Source membership flags |
| `source` | Which sources the row came from |

### HSK-inspired bands

| Band | Rank rule | Purpose |
|---|---:|---|
| Zero Beginner | 1–100 | survival pronunciation and daily phrases |
| HSK1 Core | 101–150 | first exam-oriented expansion |
| HSK2 Expansion | 151–300 | daily communication expansion |
| HSK3 Expansion | 301–600 | broader reading and speaking base |

## 🗺️ Status

Stable and reproducible. The data is generated by `scripts/build_materials.py`; deleting `data/processed/`, `content/`, `pdf/`, and `docs/*.html` and re-running the script regenerates every output. Scope for v1 deliberately excludes writing drills, stroke order, login, databases, and interactive quizzes.

## 🤝 Connect

If this dataset saves you time, **⭐ star the repo** and **[follow @shengdabai](https://github.com/shengdabai)** — more open Chinese-teaching + AI tools are shipping in public.

Related projects:
- [chinese-textbook-generator](https://github.com/shengdabai/chinese-textbook-generator) — generate Chinese textbooks
- [LinguaLens](https://github.com/shengdabai/LinguaLens) — language-learning tooling
- [chinese-mission](https://github.com/shengdabai/chinese-mission) — Chinese learning missions

## License & credits

Educational content, generated data, and dictionary-derived definitions are shared under **[CC BY-SA 4.0](LICENSE.md)** unless a source file states otherwise. Build scripts may be reused with attribution.

- English definitions derived from **[CC-CEDICT](https://www.mdbg.net/chinese/dictionary?page=cc-cedict)** (CC BY-SA 4.0).
- Pinyin fallback via **[pypinyin](https://github.com/mozillazg/python-pinyin)**.
- Raw source spreadsheets live in `data/raw/`. See **[NOTICE.md](NOTICE.md)** — verify redistribution rights before publishing a fork or commercial version that bundles them.

---

<a name="中文"></a>

# 🀄 Chinese Character Content（中文）

> 面向英语母语者的**开放、开箱即用的汉字学习数据集** —— 按词频排序、标注拼音、HSK 风格分级，CSV / JSON 格式干净。

**[English](#-chinese-character-content) | 中文**

---

## 为什么做这个

大多数汉字字表都被锁在 PDF、电子表格或 App 里，没法直接查询。零基础和 HSK 1–3 学习者真正需要的是**结构化、机器可读的数据** —— 词频排名、拼音、英文释义、例词、分级 —— 能直接导入 Anki、表格、网页应用或喂给大模型。

本仓库把三份权威汉字来源合并成一份干净、文档完整的数据集，并在其上提供「先开口说」的课程内容和可打印学习材料。一切都是**静态、开放、可复现**的 —— 无需登录、数据库或 API key。

由一名中文老师（6000+ 学员）在公开构建 AI + 中文教学工具的过程中产出。

## 这是什么

- 一份合并后的**汉字数据库**，共 15,001 字，每字带词频排名、语料覆盖率、拼音、英文释义和来源标注。
- 一份**前 600 字学习子集**，按 HSK 风格分级（零基础 → HSK3 拓展）。
- **3,500 常用字**的快速参考表与分类查找表。
- **先开口说的课程**、操练卡、4 周计划、商务短语手册、Anki 卡组和可打印 PDF —— 全部由同一份数据生成。

> 分级体系是 **HSK 风格借鉴**，并非官方 HSK 大纲的复制。词汇以**口语优先**；刻意不含书写练习与笔顺。

## ✨ 仓库结构

```
data/raw/         3 份源表（3500 字表、7000 字表、25 亿词语料）
data/processed/   10 份干净 CSV（见下表）+ build_summary.json
content/          课程 + 产品包 + 配套材料（JSON）
pdf/              5 份可打印学习包
docs/             静态 GitHub Pages 站点
scripts/          build_materials.py（一键重建全部产物）
```

### 数据集（`data/processed/`）

| 文件 | 行数 | 内容 |
|---|---:|---|
| `characters_master.csv` | 15,001 | 完整合并库 —— 3500 + 7000 + 语料，含词频、拼音、释义、来源 |
| `common_3500_quick_reference.csv` | 3,500 | 3,500 常用字的拼音优先速查表 |
| `common_3500_by_category.csv` | 3,500 | 3,500 字按学习者友好类别分组 |
| `learning_characters.csv` | 600 | 前 600 学习集，按 HSK 风格分级 |
| `anki_top_600.csv` | 600 | 前 600 字的 Anki 卡组（`front,back,tags`） |
| `lesson_vocabulary.csv` | 124 | 12 节口语优先课程的词汇 |
| `speaking_drill_cards.csv` | 15 | 提示 → 中文 → 拼音 → 英文 操练卡 |
| `business_phrasebook.csv` | 12 | 商务场景短语（带拼音 + 英文） |
| `placement_checklist.csv` | 12 | Can-do 自测分级清单 |
| `four_week_plan.csv` | 4 | 主题式 4 周口语学习计划 |

## 🧱 格式 / 技术

- **数据：** UTF-8 CSV + JSON，产物无专有格式。
- **构建：** Python 3（`pandas`、`openpyxl`、`xlrd`、`reportlab`、`pypinyin`）。
- **释义：** 构建时可下载 **CC-CEDICT** 增强；拼音回退到 `pypinyin`。
- **站点：** `docs/` 下纯静态 HTML（GitHub Pages 就绪）。

## 🚀 如何使用数据

CSV 可直接用，无需构建。

```python
import pandas as pd
df = pd.read_csv("data/processed/characters_master.csv")
top100 = df[df["frequency_tier"] == "Top 100"][["character", "pinyin", "definition_en"]]
print(top100.head())
```

**Anki**：导入 `data/processed/anki_top_600.csv`（字段 `front` / `back` / `tags`，`back` 含 HTML）。

**重新构建**：
```bash
python3 -m venv .venv
.venv/bin/python -m pip install -r requirements.txt
.venv/bin/python scripts/build_materials.py
```

## 📖 数据字典 —— `characters_master.csv`

| 列名 | 含义 |
|---|---|
| `character` | 汉字 |
| `pinyin` / `pinyin_plain` / `pinyin_initial` | 带声调拼音 / 无声调 / 首字母 |
| `definition_en` | 英文释义（来自 CC-CEDICT） |
| `example_words` | 含该字的常用词（带拼音） |
| `lookup_category` | 学习者友好类别 |
| `frequency_tier` | 词频层级（Top 100 / Top 300 …） |
| `learning_band` | HSK 风格分级 |
| `frequency_rank` | 语料词频排名 |
| `coverage_rate_pct` | 累计文本覆盖率 |
| `rank_3500` / `rank_7000` | 在 3500 / 7000 字表内的排名 |
| `in_3500` / `in_7000` / `in_corpus` | 来源归属标记 |
| `source` | 该行的来源组合 |

## 🤝 联系

如果这份数据集帮你省了时间，请 **⭐ Star** 并 **[关注 @shengdabai](https://github.com/shengdabai)** —— 更多开放的中文教学 + AI 工具正在公开构建中。

相关项目：[chinese-textbook-generator](https://github.com/shengdabai/chinese-textbook-generator) · [LinguaLens](https://github.com/shengdabai/LinguaLens) · [chinese-mission](https://github.com/shengdabai/chinese-mission)

## 许可与致谢

教育内容、生成数据与词典派生释义按 **[CC BY-SA 4.0](LICENSE.md)** 共享（除非源文件另有说明）。构建脚本可署名后复用。

- 英文释义源自 **[CC-CEDICT](https://www.mdbg.net/chinese/dictionary?page=cc-cedict)**（CC BY-SA 4.0）。
- 拼音回退使用 **[pypinyin](https://github.com/mozillazg/python-pinyin)**。
- 原始源表位于 `data/raw/`，详见 **[NOTICE.md](NOTICE.md)** —— 分发或商用前请先确认再分发权利。
