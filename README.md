# QCF4 Quran Database

[![npm](https://img.shields.io/npm/v/quran-qcf4?color=c9a84c&label=npm)](https://www.npmjs.com/package/quran-qcf4)
[![Live Demo](https://img.shields.io/badge/demo-live-2ecc8a)](https://mohamadhajjrabee.github.io/quran-qcf4/demo.html)
[![License](https://img.shields.io/badge/license-see%20repo-lightgrey)](https://github.com/MohamadHajjRabee/quran-qcf4)

> **[▶ Live Demo](https://mohamadhajjrabee.github.io/quran-qcf4/demo.html)** · Renders any Mushaf page in the browser using QCF4 fonts via CDN.

A developer-friendly Quran database using **QCF v4** (Quran Complex Font, version 4) glyph rendering for the **Hafs** recitation.

## About QCF4

QCF4 is a new, previously unpublished Quranic font based on the Madinah Mushaf (1441 AH), written by the calligrapher **Uthman Taha** and produced by the **King Fahd Complex** in Madinah.

It is an improved and modern edition of the second Madinah font (QCF2, 2013), with the following enhancements:

1. Improved quality of letter shapes and diacritical marks
2. Removal of whitespace gaps between words
3. Reduced font file count to just **47 files** (down from 604 in QCF1/QCF2 which required one font per page)
4. Updated verse-end marker design

> خط قرآن مصحف المدينة النبوية ١٤٤١هـ
> الإصدار الرابع — رواية حفص عن عاصم.
> عُثمان طه. مُجمّع الملك فهد — QCF4

Font version by **Ahmad ElGharib** — [Telegram Channel](https://t.me/quranfont)

## What's Included

```
├── pages/          # 604 JSON files (one per Mushaf page)
│   ├── 001.json
│   ├── 002.json
│   └── ...
├── fonts/          # 47 QCF4 Hafs font files + BSML header font
│   ├── QCF4_Hafs_01_W.ttf
│   ├── QCF4_Hafs_02_W.ttf
│   ├── ...
│   └── QCF4_QBSML.ttf
├── fonts-woff2/    # 47 QCF4 Hafs woff2 font files + BSML header font
│   ├── QCF4_Hafs_01_W.woff2
│   ├── QCF4_Hafs_02_W.woff2
│   ├── ...
│   └── QCF4_QBSML.woff2
├── index.json      # Master index with chapter metadata
├── verses.json     # Verse-key → page/line lookup index
└── font-map.json   # Page number → font name mapping
```

## Page JSON Schema

Each `pages/NNN.json` file represents one Mushaf page:

```json
{
  "page": 1,
  "font": "QCF4_Hafs_01",
  "surahs": [
    {
      "id": 1,
      "name": "Al-Fatihah",
      "name_arabic": "الفاتحة",
      "verse_start": 1,
      "verse_end": 7
    }
  ],
  "lines": [
    {
      "line": 1,
      "words": [
        {
          "code": 61696,
          "char": "\uF100",
          "font": "QCF4_QBSML",
          "text": "سُورَةُ الفَاتِحَةِ",
          "type": "surah_header",
          "sura": 1
        }
      ]
    },
    {
      "line": 2,
      "words": [
        {
          "code": 61696,
          "char": "\uF100",
          "font": "QCF4_Hafs_01",
          "text": "بِسْمِ",
          "type": "word",
          "verse_key": "1:1",
          "position": 1
        },
        ...
      ]
    }
  ]
}
```

### Word Types

| Type | Description |
|------|-------------|
| `word` | Quranic word — has `verse_key` and `position` |
| `end` | Verse-end marker (circled number) — has `verse_key` and `position` |
| `surah_header` | Surah title banner — has `sura`, uses `QCF4_QBSML` font |
| `bismillah` | Bismillah glyph — has `sura`, uses `QCF4_Hafs_01` font |
| `quarter` | Quarter-hizb marker (۞) |

## Verse Index (`verses.json`)

Quick lookup to find any verse by key:

```json
{
  "1:1": {
    "page": 1,
    "lines": [
      { "line": 2, "word_start": 1, "word_end": 5 }
    ]
  },
  "2:255": {
    "page": 42,
    "lines": [
      { "line": 6, "word_start": 1, "word_end": 8 },
      { "line": 7, "word_start": 9, "word_end": 19 },
      { "line": 8, "word_start": 20, "word_end": 28 }
    ]
  }
}
```

A verse can span multiple lines. Each entry tells you which page, which lines, and the word positions on each line.

## Font Map (`font-map.json`)

Maps each page number to its font name:

```json
{
  "1": "QCF4_Hafs_01",
  "2": "QCF4_Hafs_01",
  ...
  "604": "QCF4_Hafs_47"
}
```

## How to Render

### 1. Load the Font

Each page uses one main font. Load it via CSS `@font-face`:

```css
@font-face {
  font-family: "QCF4_Hafs_01";
  src: url("fonts/QCF4_Hafs_01_W.ttf");
}
@font-face {
  font-family: "QCF4_QBSML";
  src: url("fonts/QCF4_QBSML.ttf");
}
```

### 2. Render Words

Each word has a `char` field containing the Unicode character that maps to the correct glyph in the font:

```html
<span style="font-family: 'QCF4_Hafs_01'; font-size: 28px;">&#xF100;</span>
```

Or using JavaScript:

```js
const span = document.createElement("span");
span.style.fontFamily = word.font;
span.textContent = word.char;
```

### 3. Layout

Words are organized by lines as they appear on the physical Mushaf page. Render each line as a row (RTL direction), and each word as an inline element within that row.

## CDN Usage (No Install Required)

You can use the data and fonts directly via jsDelivr — no npm install needed:

```html
<!-- Load a font -->
<style>
  @font-face {
    font-family: "QCF4_Hafs_01";
    src: url("https://cdn.jsdelivr.net/gh/MohamadHajjRabee/quran-qcf4@main/fonts-woff2/QCF4_Hafs_01_W.woff2");
  }
</style>
```

```js
// Fetch page data
const page = await fetch(
  "https://raw.githubusercontent.com/MohamadHajjRabee/quran-qcf4/main/pages/001.json"
).then(r => r.json());
```

> **Note:** Use `raw.githubusercontent.com` for JSON data (always up to date) and `cdn.jsdelivr.net` for font files (cached for performance).

## Index (`index.json`)

Contains metadata for all 114 chapters:

```json
{
  "meta": {
    "schema_version": "1.0.0",
    "total_pages": 604,
    "total_chapters": 114,
    "total_verses": 6236,
    "font_count": 47
  },
  "chapters": [
    {
      "id": 1,
      "name": "Al-Fatihah",
      "name_arabic": "الفاتحة",
      "revelation_place": "makkah",
      "revelation_order": 5,
      "bismillah_pre": false,
      "verses_count": 7,
      "pages": [1, 1],
      "translated_name": "The Opener"
    }
  ]
}
```

## Credits

- **QCF4 Fonts**: Madinah Mushaf 1441 AH, calligraphy by **Uthman Taha**, produced by the **King Fahd Quran Complex** (مُجمّع الملك فهد لطباعة المصحف الشريف). QCF4 font version by **Ahmad ElGharib** — [Telegram Channel](https://t.me/quranfont)
- **Chapter metadata**: Derived from [Quran.com](https://quran.com) open data

## License

The font files are provided for Quranic rendering purposes. Please respect the original creator's terms of use.
