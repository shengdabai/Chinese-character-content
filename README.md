# Chinese Character Content

Static learning materials for English-speaking beginners, HSK 1-3 learners, and business Chinese starters.

## Outputs

- `data/processed/characters_master.csv`: merged character database from the 3500 list, 7000 list, and corpus frequency file.
- `data/processed/learning_characters.csv`: top 600 learning characters grouped by beginner/HSK-inspired bands.
- `data/processed/common_3500_quick_reference.csv`: foreign-learner lookup sheet for the 3500 common characters.
- `data/processed/common_3500_by_category.csv`: the same 3500 characters grouped by learner-friendly categories.
- `content/lessons.json`: speaking-first lesson content.
- `content/product_packs.json`: productized learning pack ideas and positioning.
- `content/learning_materials_companion.json`: teacher guide, learner guide, 4-week plan, drill cards, business phrases, and placement checklist.
- `pdf/Chinese_Character_Content_Study_Pack.pdf`: printable study pack.
- `pdf/Master_Character_List_Top_600.pdf`: printable top-600 character reference.
- `pdf/Common_3500_Character_Quick_Reference.pdf`: pinyin-first 3500-character quick lookup PDF.
- `pdf/Sellable_Learning_Packs.pdf`: sellable content-pack positioning sheet.
- `pdf/Learning_Materials_Companion.pdf`: teacher-ready and learner-ready companion materials.
- `docs/index.html`: GitHub Pages static website.

## Build

```bash
python3 -m venv .venv
.venv/bin/python -m pip install -r requirements.txt
.venv/bin/python scripts/build_materials.py
```

## Local Preview

Open `docs/index.html` in a browser, or run:

```bash
python3 -m http.server 8080 -d docs
```

## Design Notes

The level system is HSK-inspired rather than an official HSK syllabus copy:

| Band | Character rank rule | Purpose |
|---|---:|---|
| Zero Beginner | 1-100 | survival pronunciation and daily phrases |
| HSK1 Core | 101-150 | first exam-oriented expansion |
| HSK2 Expansion | 151-300 | daily communication expansion |
| HSK3 Expansion | 301-600 | broader reading and speaking base |

Vocabulary and example lessons are speaking-first. Writing practice and stroke order are intentionally excluded.

## Data Sources

- Local source files in `data/raw/`.
- English definitions are enriched from CC-CEDICT when the build script can download it.
- Pinyin fallback is generated with `pypinyin`.

See `NOTICE.md` for attribution and license notes.
