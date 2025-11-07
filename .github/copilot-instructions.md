## Repository snapshot

This repository contains a small bash-based automated backup system. Key files and directories:

- `bin/backup.sh` — main script (strict bash: `set -euo pipefail`).
- `config/config/backup.config` — user-editable settings (keep counts, exclude patterns, report paths).
- `backups/` — stores created archives under `daily/`, `weekly/`, `monthly/`.
- `logs/` and `reports/` — where runtime logs and summary reports are expected.

## What an AI helper should know (quick)

- The script `bin/backup.sh` determines backup type (daily/weekly/monthly) by date and writes archives
  to `$SCRIPT_DIR/backups/<type>/backup-<timestamp>.tar.gz`.
- Usage patterns found in the repo:
  - Create a backup: `./bin/backup.sh /path/to/source`
  - Dry-run: `./bin/backup.sh --dry-run /path/to/source`
  - List: `./bin/backup.sh --list`
  - Restore: `./bin/backup.sh --restore <file> --to <folder>`
- The script uses `tar -czf` and `tar -xzf` for compression and extraction.

## Project-specific conventions and gotchas

- Strict bash mode is used (`set -euo pipefail`) — avoid introducing commands that rely on unset variables.
- `SCRIPT_DIR` is computed as the repo root relative to `bin/` via `BASH_SOURCE`; prefer absolute paths when editing.
- `config/config/backup.config` exists and documents `DAILY_KEEP`, `WEEKLY_KEEP`, `MONTHLY_KEEP`, `EXCLUDE_PATTERNS`, etc.
  - Note: `bin/backup.sh` currently defines `CONFIG_FILE` but does not source it. If you update behavior to use
    config settings, ensure the script sources the config early (e.g., `source "$CONFIG_FILE"`) and validates values.
- Backups are placed in `backups/<daily|weekly|monthly>`; tests and features should operate on those folders.

## Helpful examples to use in edits and tests

- Enable dry-run behavior in unit-like tests by calling:
  `./bin/backup.sh --dry-run /some/test/source`
- Verify restore flow with a small tarball:
  `./bin/backup.sh --restore backups/daily/backup-YYYY-MM-DD_HH-MM-SS.tar.gz --to ./tmp-restore`

## Developer workflows (how humans run things)

- This is a shell-first project — run scripts in a POSIX shell (Linux/macOS) or via WSL/Git Bash on Windows.
- Build/CI: There is no build system. Tests (if added) should be simple shell scripts or a small test harness.

## What to watch for when changing code

- Preserve logging format: `log()` writes timestamps and appends to `logs/backup.log` using `tee -a`.
- When adding features that change the archive layout or timestamps, update README examples and any report-generation logic.
- If you add config sourcing, validate and provide defaults so missing config doesn't break `set -u`.

## Files to reference when making edits

- `bin/backup.sh` — canonical behavior (args parsing, dry-run, list, restore, timestamping).
- `config/config/backup.config` — desired config shape and defaults.
- `restore/automated-backss/README.md` and top-level `README.md` — user-facing docs to keep in sync.

## Minimal acceptance criteria for PRs

- New shell changes must keep `set -euo pipefail` compatibility and not introduce silent failures.
- CLI behavior must match examples in `README.md` or the README should be updated.
- If config usage is added, include a short snippet showing `source "$CONFIG_FILE"` and a validation block.

If anything here is unclear or you want the file adjusted to emphasize other patterns, tell me which areas to expand.
