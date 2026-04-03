# Codex Global Skills Backup

This directory contains a git-managed mirror of the Codex global skill trees from this machine.

## Sources

- `C:\Users\Demonthorn\.codex\superpowers\skills` -> [skills/superpowers](/D:/Backend/nova/skills/superpowers)
- `C:\Users\Demonthorn\.codex\skills` -> [skills/](/D:/Backend/nova/skills)

## Mirror Policy

- `skills/superpowers` is mirrored from the global `superpowers/skills` tree.
- Each top-level directory under the global `skills` tree is mirrored into the matching top-level directory under [skills](/D:/Backend/nova/skills).
- Repo-local directories that do not exist in the global source, such as [skills/blazy](/D:/Backend/nova/skills/blazy), are left intact.

## Last Backup

- Date: `2026-03-19`
- Time zone: `Asia/Saigon`
- Method: per-directory mirror from global Codex paths into repo paths

## Why

This mirror exists so global skill changes can be reviewed, versioned, and restored with git instead of living only in the Codex home directory.
