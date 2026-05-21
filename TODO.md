# TODO

## Active

## Done
- [x] **Consider PKGBUILD for `nemesis_repo` packaging** — _closed 2026-05-21 (already covered)._ The sibling [plymouth-theme-kiro](/home/erik/EDU/plymouth-theme-kiro/) already has a PKGBUILD in `~/EDU-PKG-BUILD/edu-pkgbuild/plymouth-theme-kiro/` shipping into `nemesis_repo`. No separate packaging needed for this repo.
- [x] **Write install instructions in README.md** — _done 2026-05-21._ Full Arch/Kiro README: requirements, 5-step install (copy theme, add `plymouth` hook to `mkinitcpio.conf`, `plymouth-set-default-theme -R startrek`, `quiet splash` in GRUB cmdline, reboot), live-preview via `plymouthd --debug` + `plymouth --show-splash`, uninstall, packaging pointer.
- [x] **Verify theme renders correctly during Kiro ISO boot sequence** — _confirmed by Erik (closed 2026-05-21)._

## Backlog
- [ ] Add alternative color variants
