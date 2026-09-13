## Agent Memories

### OpenSpec SDD Setup (Fission-AI/OpenSpec port, per-project installation model)

**Installation model** (mirrors `npm install -g @fission-ai/openspec` + `openspec init`):
- **Global ("the package")**: `.assistant/skills/openspec-sdd/` -- installed once, shared across projects
  - `scripts/` -- init.py, validate.py, status.py, sync.py, archive.py, list_changes.py
  - `schemas/spec-driven/` -- schema.yaml + templates/ (identical to Fission-AI original)
  - `SKILL.md` -- index/router with CLI-to-script mapping
- **Command skills** (global, one per command like original `skills/<cmd>/SKILL.md`):
  - openspec-propose, openspec-explore, openspec-apply, openspec-update
  - openspec-verify, openspec-sync, openspec-archive
  - openspec-new-change, openspec-continue-change, openspec-ff-change
  - openspec-onboard, openspec-bulk-archive-change
  - Total: 12 command skills (all 12 from original repo ported)
- **Per-project** (created by `init.py` for each project, like `openspec init`):
  - `<project>/openspec/config.yaml` -- project-specific context and rules
  - `<project>/openspec/specs/` -- source of truth for that project
  - `<project>/openspec/changes/` -- active and archived changes

**Root resolution**: When user invokes /opsx:*, find the nearest `openspec/` relative to the asset they're working on. If ambiguous, ask which project.

**No hardcoded paths**: All skills use `<PROJECT_ROOT>/openspec` and `<SCRIPTS>` placeholders resolved at runtime.

**Demo project**: `~/demo-openspec-project/` -- initialized with OpenSpec, ready for `/opsx:propose`.

**Distribution package**: `/Workspace/Shared/openspec-sdd-package/` -- shared location for other users
  - `install.py` -- installer (install, --force update, --uninstall, --status, --dry-run)
  - `skills/` -- 13 skill directories (copied to user's .assistant/skills/ on install)
  - `README.md` -- quick start guide
  - Other users run: `python /Workspace/Shared/openspec-sdd-package/install.py`
