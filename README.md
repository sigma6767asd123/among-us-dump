# Cleaned Among Us IL2CPP dump

Large files have been split so every individual file is below GitHub's 25 MB upload limit.
No dump records were intentionally removed.

## Layout
- `script_<section>_<part>.json`: valid JSON files containing one section each. Parts with the same section can be combined by merging their arrays.
- `*.partNNN`: byte-preserving line-boundary chunks of the original large text/header files. Join chunks in numeric order to reconstruct the original file.
- Small files are unchanged.

## Reconstruct split text files (Python)
Run from this directory:
```python
from pathlib import Path
import re
for first in Path('.').glob('*.part001'):
    stem = re.sub(r'\.part001$', '', first.name)
    parts = sorted(Path('.').glob(stem + '.part*'))
    Path(stem).write_bytes(b''.join(p.read_bytes() for p in parts))
```

## Recombine `script_*.json`
Each JSON file contains one top-level array. For each section name, concatenate the arrays from its numbered parts into one array under that section key.
