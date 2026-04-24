# Product Plan

## Background

The source files are character-oriented. Foreign learners need a path from characters to words, sentences, and speaking tasks:

```text
Character frequency
        |
        v
Learning bands
        |
        v
Words and phrases
        |
        v
Sentences
        |
        v
Speaking tasks
```

## Requirements

| Priority | Requirement |
|---|---|
| Must | Create PDF and static web materials |
| Must | Use English as the teaching language |
| Must | Support zero beginners, HSK 1-3 learners, and business users |
| Must | Include pinyin and English meaning |
| Must | Focus on reading aloud and speaking |
| Should | Use HSK-inspired level sizes |
| Should | Publish through GitHub Pages |
| Won't | No writing drills, stroke order, login, database, or interactive quiz in v1 |

## Recommended Architecture

```text
Excel files
   |
   v
scripts/build_materials.py
   |
   +--> data/processed/*.csv
   +--> content/lessons.json
   +--> pdf/*.pdf
   +--> docs/*.html
```

## Acceptance Criteria

| Area | Criteria |
|---|---|
| Data | Source files are read, merged, deduplicated, and ranked |
| Content | Lessons include Chinese, pinyin, English, and speaking tasks |
| PDF | Study pack and top-600 list open locally |
| Web | Static site opens locally and can run on GitHub Pages |
| Maintenance | Re-running the build script regenerates outputs |

## Rollback

All generated outputs can be deleted and recreated with:

```bash
.venv/bin/python scripts/build_materials.py
```
