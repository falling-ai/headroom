# Windows Compatibility Log

Tracks which upstream `headroomlabs-ai/headroom` version/commit our Windows
workaround docs and scripts (`wiki/windows-deployment.md`,
`scripts/windows/`) were last validated against, and what changed.

This is maintained on the `windows-support` branch of this fork
(`falling-ai/headroom`), forked from `headroomlabs-ai/headroom`.

| Date | headroom-ai (PyPI) | Upstream commit | Notes |
|---|---|---|---|
| 2026-07-12 | 0.31.0 (win_amd64 wheel) | `2c9eb7c5` (`v0.31.0-93-g2c9eb7c5`, main) | First validation pass. Confirmed: win_amd64 wheel installs without MSVC; `persistent-service`/`persistent-task` both fail with Access Denied on a standard (non-admin) account; `AtLogOn` Scheduled Task workaround confirmed working; hook-based `ensure` race (see wiki known issues) reproduced on headroom-ai 0.27.0 (source build) prior to upgrading, not re-tested against 0.31.0 under load. |

## What to re-check on each upstream bump

- [ ] `pip install "headroom-ai[proxy]"` still resolves a `win_amd64` wheel (`pip download` or check PyPI file list).
- [ ] `headroom install apply --preset persistent-service` / `--preset persistent-task` on a **standard** (non-admin) Windows account — re-test whether the Access Denied gap has closed.
- [ ] Status of [headroomlabs-ai/headroom#822](https://github.com/headroomlabs-ai/headroom/pull/822) (Windows ONNX compression wedge) — if merged, re-test the hook-based `ensure` race under a slow-compression request.
- [ ] `docs/platform-feature-matrix.json` upstream — diff the `windows` status/gap text for `install_apply_python`, `install_windows_service`, `single_instance_start`, `compression_fail_open`.
