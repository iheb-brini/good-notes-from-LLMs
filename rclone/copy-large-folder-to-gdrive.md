# Copy a large local folder to Google Drive with rclone

**Context:** 22 GB / 555 files in a local `tmp/` folder on a Linux server, to be moved off the disk to Google Drive.

## Why rclone

- Handles tens of GB: resumable, parallel transfers, chunked uploads.
- `rclone check` verifies the upload against the local copy before anything is deleted.
- Deletes nothing unless you explicitly ask (`move` / `rm`).
- Alternatives that don't fit: the Drive web UI (impractical over SSH / for 22 GB), Drive API connectors (small files only), `gdown` (download only).

## 1. One-time setup: create the Drive remote

```bash
rclone config
```

- `n` → new remote, name it `gdrive`, storage type `drive`
- Leave `client_id` / `client_secret` blank (create your own OAuth client ID later if you get rate-limited or uploads are slow)
- scope `1` (full access)
- **"Use auto config?"**
  - Machine with a browser → `y`
  - Headless server over SSH → `n`, then on a laptop with rclone and a browser run:
    ```bash
    rclone authorize "drive"
    ```
    and paste the printed token back into the server prompt.

Check it works:

```bash
rclone listremotes          # should show gdrive:
rclone lsd gdrive:          # lists top-level Drive folders
```

## 2. Copy → check → delete (safe order)

```bash
SRC=tmp/folders_for_commonform
DST=gdrive:commonforms/folders_for_commonform

# upload (can be re-run; already-uploaded files are skipped)
rclone copy "$SRC" "$DST" --progress --transfers 8 --drive-chunk-size 128M

# confirm every file arrived intact
rclone check "$SRC" "$DST"

# only after the check passes, free the local space
rm -rf "$SRC"
```

One-step alternative (deletes each local file after it's uploaded):

```bash
rclone move "$SRC" "$DST" --progress --delete-empty-src-dirs
```

## 3. Getting it back

```bash
rclone copy "$DST" "$SRC" --progress      # download again
rclone mount gdrive: ~/gdrive &           # or open Drive like a local folder (needs FUSE)
```

## Tips

- Run long uploads inside `tmux` / `screen` (or with `nohup`) so they survive an SSH disconnect.
- Google Drive limits uploads to **750 GB per day** per account.
- `--transfers 8 --drive-chunk-size 128M` speeds things up on a good connection. Each transfer uses about one chunk of RAM.
- Distro packages can be old (this machine had v1.53 from 2020). Get the latest with:
  ```bash
  curl https://rclone.org/install.sh | sudo bash
  ```
- `--dry-run` on any command shows what would happen without doing it.
