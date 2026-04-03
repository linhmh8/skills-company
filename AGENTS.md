# Skills Repository Guide

This repository is a git-managed skills catalog for Codex.

## Structure

The repository is intentionally nested for ownership and grouping:

- `blazy/` contains org-owned ecosystem skills
- `superpowers/` contains shared process skills
- root-level skill folders such as `nova-bigbang/` contain repo-specific skills

These buckets are organizational only.
They are not a signal that global Codex should keep the same nested shape.

## Skill Identity

Treat any directory containing `SKILL.md` as one skill.
The canonical skill name is the skill directory name.

When comparing skills, compare by skill name and full directory contents.
Do not compare by repo tree shape.

## Global Codex Rule

Global Codex homes must stay flat:

- WSL example: `/root/.codex/skills/<skill-name>/`
- Windows example: `/mnt/c/Users/<user>/.codex/skills/<skill-name>/`

Do not sync repo buckets like `blazy/` or `superpowers/` into global Codex as extra nesting.
Flatten every repo skill into `<codex-home>/skills/<skill-name>/`.

## Compare Rule

When comparing repo skills to a Codex home:

1. discover repo skills recursively by finding `SKILL.md`
2. discover Codex-home skills recursively by finding `SKILL.md`
3. group both sides by skill name
4. compare each skill by full tree content, not by parent folders

If the same skill exists multiple times in a Codex home, that is a flattening problem.
Still compare by name first, then report duplicate locations.

## Sync Rule

When syncing this repo into a Codex home:

1. discover all repo skills recursively
2. write each repo skill into `<codex-home>/skills/<skill-name>/`
3. for synced skills, remove duplicate copies elsewhere in the Codex home so the target stays flat
4. do not touch `.system/`
5. do not delete unrelated global-only skills unless the user explicitly asks

## Path Style

Inside repo skill content, prefer skill names or repo-relative paths.
Do not hardcode machine-local paths such as `/root/.codex/...` when a name or relative path is enough.

## Intent

Repo structure is allowed to be nested.
Global Codex structure is not.

The agent must preserve both truths at the same time:

- repo keeps its ownership buckets
- global Codex gets a flat installed skill tree
