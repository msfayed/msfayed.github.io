# Ultimate Guide: Syncing Millions of Files to Cloudflare R2 Using Rclone

This guide details how to install, configure, and optimize **Rclone** to handle high-performance, one-way mirroring of millions of files into an empty or partially full Cloudflare R2 bucket without crashing or blowing up your API costs.

---

## 1. Installation

Install Rclone on your system using the official one-liners.

### Linux & macOS
```bash
curl https://rclone.org | sudo bash
```

### Windows (PowerShell)
```powershell
winget install rclone.rclone
```

---

## 2. Configuration (`rclone.conf`)

Run `rclone config` to set up your remote, or directly edit your Rclone configuration file (`~/.config/rclone/rclone.conf` on Linux/macOS or `%USERPROFILE%\.config\rclone\rclone.conf` on Windows).

Add the following block to create a remote named `r2`:

```ini
[r2]
type = s3
provider = Cloudflare
access_key_id = <your_r2_access_key_id>
secret_access_key = <your_r2_secret_access_key>
endpoint = https://<your_account_id>.r2.cloudflarestorage.com
region = auto
acl = private
no_check_bucket = true
```

> **Crucial Setting:** `no_check_bucket = true` is highly recommended. It prevents Rclone from making a top-level bucket validation request, which is required if your Cloudflare API token is restricted to a single bucket. It also saves unnecessary API calls.

---

## 3. Phase 1: Fast Initial Upload (Empty Target Bucket)

If your target Cloudflare R2 bucket is **completely empty**, you can bypass all destination checking and folder scanning. This eliminates initialization lag and lowers Class B API costs.

### The Pure Ingestion Command
```bash
rclone copy /path/to/local/folder r2:your-bucket-name \
  --no-check-dest \
  --no-traverse \
  --transfers 64 \
  --checkers 1 \
  --buffer-size 32M \
  --progress
```

### Flag Breakdown for Empty Buckets:
* **`--no-check-dest`**: The ultimate shortcut. Tells Rclone to skip looking at R2 entirely and assume every local file is missing, launching uploads instantly.
* **`--no-traverse`**: Stops Rclone from trying to map out the remote bucket before uploading.
* **`--transfers 64`**: Boosts concurrent uploads to maximize bandwidth. (Lower to `16` or `32` if files are mostly multi-gigabyte videos/archives to prevent disk thrashing).
* **`--checkers 1`**: Minimizes background checking overhead since `--no-check-dest` disables file comparison.
* **`rclone copy`**: Safer than `sync` for clean initialization since an empty bucket has no files to delete.

---

## 4. Phase 2: Resuming an Interrupted Sync (Partially Full Bucket)

If your transfer fails, drops network, or you add new files later, **do not use `--no-check-dest` or `--no-traverse`**. Doing so will cause Rclone to blindly re-upload everything or issue millions of expensive individual API requests.

### The Optimized Resume Command
```bash
rclone copy /path/to/local/folder r2:your-bucket-name \
  --fast-list \
  --size-only \
  --transfers 64 \
  --checkers 64 \
  --buffer-size 32M \
  --progress
```

### Flag Breakdown for Resuming:
* **`--fast-list`**: Fetches the existing remote file architecture in huge chunks of 1,000 files per API call. This saves hours of metadata prep time and thousands of dollars in Class B API operations.
* **`--size-only`**: Instructs Rclone to compare *only* the file size. If the sizes match, it assumes the files match and skips them, ignoring deceptive timestamp mismatches.
* **`--checkers 64`**: Deploys a large team of parallel background workers to rapidly scan and map the local files against the remote database.

---

## 5. Targeting a Specific Sub-Folder Only

If your millions of files are divided into structured sub-directories (e.g., by year, category, or user ID), you can dramatically speed up operations by isolated syncs. Targeting a sub-folder cuts scanning down to just that path, bypassing the rest of your dataset entirely.

### The Sub-Folder Command
```bash
rclone copy /path/to/local/folder/sub-directory r2:your-bucket-name/sub-directory \
  --no-traverse \
  --size-only \
  --transfers 64 \
  --checkers 64 \
  --progress
```

### Strategic Rules for Sub-Folders:
* **Append the sub-folder to both paths:** Ensure you match the paths exactly on both sides (`/path/to/local/folder/images/2026` to `r2:your-bucket-name/images/2026`). If you omit the destination suffix, Rclone will dump the *contents* of your local folder directly into the bucket root.
* **When to combine with `--no-traverse`:** You can safely bring back `--no-traverse` here **only** if you know this specific sub-folder is completely empty on R2, or contains very few files. This prevents Rclone from running a full inventory scan.
* **When to drop `--no-traverse`:** If you are resuming a broken upload *inside* that specific sub-folder, drop `--no-traverse` and add `--fast-list` so Rclone lists only that specific directory's existing remote files efficiently.

---

## 6. Ensuring Uninterrupted Background Execution via SSH (`nohup`)

If you are managing your server over SSH, closing your terminal app sends a hang-up signal (`SIGHUP`) that will instantly kill your upload. To prevent this without installing third-party tools like tmux, wrap your command in a self-healing `while` loop and execute it using **`nohup`** (No Hang Up).

### The Background Process Loop

Run the following command exactly as written. This pushes the loop directly into the background, logs stdout to `nohup.out` (which we redirect to empty space), and tells Rclone to auto-retry every 10 seconds if a network error occurs:

```bash
nohup sh -c 'while ! rclone copy /path/to/local/folder r2:your-bucket-name --fast-list --size-only --transfers 64 --checkers 64 --buffer-size 32M --low-level-retries 10 --retries 10 --log-file=/path/to/rclone.log --log-level=INFO; do echo "Rclone disconnected. Retrying in 10 seconds..."; sleep 10; done; echo "Upload fully complete!"' > /dev/null 2>&1 &
```

* **`nohup ... &`**: Instructs the operating system to completely ignore terminal disconnections and run the execution thread completely detached in the background.
* **`> /dev/null 2>&1`**: Prevents the terminal from generating a bloated `nohup.out` file on your local storage, sending all standard output to a black hole since Rclone handles its own logging.
* **`--log-file=/path/to/rclone.log --log-level=INFO`**: Generates a clean, local file to securely keep track of file transfer progress, status errors, and final speed metrics.

### Checking on the Script Status After Disconnecting

Once you run the command, you can safely log out of your SSH session by typing `exit`. When you log back in later, use these diagnostic tools to monitor your progress:

#### 1. Live Progress Monitoring
Stream your active Rclone transfers directly inside your terminal window using `tail`:
```bash
tail -f /path/to/rclone.log
```
*(Press `Ctrl + C` to stop viewing the log file stream; this will **not** kill the background upload).*

#### 2. Verify the Process is Alive
Check if the server is still actively running your Rclone workload:
```bash
ps aux | grep rclone
```
If it returns an active process line containing your copy flags, it is still running. If it returns nothing (other than your own grep command), the script loop has completely finished processing your files.
