---
title: Example - inspect, process, and move data
---

In this example you’ll (1) inspect local files, (2) run a small Python check with **rasterio**, and (3) interact with a public S3 bucket. Your environment has **Python**, **AWS CLI**, and **rclone** available (Python starts with a minimal standard library).

## 1) Inspect the local rasters

List files and sizes:

```terminal:execute
command: ls -lh data/*.tif
```

Copy the same command (if you prefer pasting yourself):

```workshop:copy
text: ls -lh data/*.tif
```

Compute quick checksums to verify integrity:

```terminal:execute
command: sha256sum data/*.tif
```

Copy instead of execute:

```workshop:copy
text: sha256sum data/*.tif
```

## 2) Python example with rasterio

First, install `rasterio` (user-site install):

```terminal:execute
command: python -m pip install --user --upgrade pip && python -m pip install --user rasterio
```

Create a Python file `analyze.py` in your home directory:

```editor:append-lines-to-file
file: ~/analyze.py
text: |
  from pathlib import Path
  try:
      import rasterio
  except Exception as exc:
      raise SystemExit(f"Rasterio not available: {exc}\nTry: python -m pip install --user rasterio") from exc

  def describe(path: Path):
      with rasterio.open(path) as ds:
          print(f"{path.name}: {ds.count} band(s), {ds.width}x{ds.height}, {ds.dtypes}, crs={ds.crs}")
          print(f"  bounds: {ds.bounds}")
          print(f"  transform: {ds.transform}\n")

  if __name__ == "__main__":
      for p in sorted(Path("data").glob("*.tif")):
          describe(p)
```

Open it in the embedded editor:

```editor:open-file
file: ~/analyze.py
line: 1
```

Run the script:

```terminal:execute
command: python ~/analyze.py
```

Copy the run command:

```workshop:copy
text: python ~/analyze.py
```

## 3) Verify CLI tools and browse a public S3 dataset

Check installed versions:

```terminal:execute
command: aws --version
```

```terminal:execute
command: rclone version
```

### List public data with AWS CLI (anonymous, no signing)

The following lists a public bucket path:

```terminal:execute
command: aws s3 ls s3://esa-worldcover-s2/rgbnir/2021/S22/ --no-sign-request --region eu-central-1
```

Copy instead:

```workshop:copy
text: aws s3 ls s3://esa-worldcover-s2/rgbnir/2021/S22/ --no-sign-request --region eu-central-1
```

### List the same path with rclone (anonymous config)

Create rclone config (anonymous S3 access):

```terminal:execute
command: mkdir -p ~/.config/rclone
```

```editor:append-lines-to-file
file: ~/.config/rclone/rclone.conf
text: |
  [aws-public]
  type = s3
  provider = AWS
  region = eu-central-1
  access_key_id =
  secret_access_key =
```

List the path via rclone:

```terminal:execute
command: rclone ls aws-public:esa-worldcover-s2/rgbnir/2021/S22/
```

Copy instead:

```workshop:copy
text: rclone ls aws-public:esa-worldcover-s2/rgbnir/2021/S22/
```

## Try it yourself: copy data

Copy (download) a subset to a local folder with **AWS CLI** (anonymous):

```workshop:copy-and-edit
text: |
  mkdir -p ./worldcover/S22 && \
  aws s3 cp s3://esa-worldcover-s2/rgbnir/2021/S22/ ./worldcover/S22/ \
    --no-sign-request --region eu-central-1 --recursive --exclude "*" --include "*.tif"
```

Or with **rclone**:

```workshop:copy-and-edit
text: |
  rclone copy aws-public:esa-worldcover-s2/rgbnir/2021/S22/ ./worldcover/S22/ --progress
```
