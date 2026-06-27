# meikipop-vietnamese-dict

Scripts to build a Vietnamese–Japanese dictionary package for [Meikipop](https://github.com/bluecat76/meikipop) from Yomitan-format dictionary zips.

## Background

The Vietnamese definitions are produced by machine-translating (MTL) the English glosses from [yomidevs/jmdict-yomitan](https://github.com/yomidevs/jmdict-yomitan). There is no official Vietnamese release — you need to generate your own `jmdict_vietnamese_plain.zip` by translating the English Yomitan zip. Accuracy varies accordingly.

VNDB Names (`VNDB_Names.zip`) provides Japanese names from [VNDB](https://vndb.org/), merged on top of JMdict as a secondary source. It remains in English — proper names don't need translation.

## What's in this repo

| File | Description |
|------|-------------|
| `import_yomitan_dict_text.py` | Converts one or more Yomitan `.zip` dictionaries into Meikipop's `dictionary.pkl`. Definitions are flattened to plain text. **This is the main script.** |
| `import_yomitan_dict_html.py` | Same as above but preserves HTML formatting from structured-content entries. |
| `VNDB_Names.zip` | Yomitan-format VNDB names dictionary, merged after JMdict Vietnamese. |

The `jmdict_vietnamese_plain.zip` source file is not included in this repo. You need to produce it yourself by MTL-ing the English JMdict Yomitan zip from [yomidevs/jmdict-yomitan releases](https://github.com/yomidevs/jmdict-yomitan/releases) into Vietnamese.

## Requirements

- Python 3.10+
- Meikipop installed (the scripts import from `meikipop.utils.paths`)

Set `PYTHONPATH` to Meikipop's `src` directory before running.

## Usage

```powershell
cd path\to\meikipop-2.x.x
$env:PYTHONPATH='path\to\meikipop-2.x.x\src'

python import_yomitan_dict_text.py jmdict_vietnamese_plain.zip VNDB_Names.zip -o "$env:LOCALAPPDATA\meikipop\dictionary.pkl"
```

Multiple zips are merged into one pickle in the order given. Restart Meikipop after rebuilding.

## Updating the dictionary

1. Download the latest English JMdict zip from [yomidevs/jmdict-yomitan releases](https://github.com/yomidevs/jmdict-yomitan/releases)
2. MTL the English glosses to Vietnamese using an LLM (e.g. ChatGPT, Gemini) to produce a new `jmdict_vietnamese_plain.zip` — batch-translating the gloss strings gives much better results than naive machine translation tools
3. Run the rebuild command above
4. Restart `meikipop.exe`
