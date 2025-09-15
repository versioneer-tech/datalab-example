# Overview

Welcome! These pages guide you through a short, hands‑on workflow using a simple folder layout:

- `workshop/content/` contains the instructions you are reading right now.
- `data/` contains two GeoTIFF files used in the example.

## Copying commands into the terminal

- Most commands are shown in code blocks. You can select the block and copy it, then paste into the terminal (on most systems: **Ctrl+Shift+V** or **Cmd+V**).
- If a command in the docs starts with a prompt symbol like `$`, don’t include the `$` when pasting.
- Multi‑line commands use a trailing `\` for line continuation. You can paste them as a single block.

### Basic orientation

Open a terminal and list the sample files:

```sh
ls -lh data/*.tif
```

Placeholders in commands look like `<YOUR_BUCKET>` or `<YOUR_REGION>`. Replace them with values that make sense for your setup.
