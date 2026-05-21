# Changelog

## 2026.05.21

### What Changed
- README.md fleshed out from a single placeholder line to a full install/usage guide. Documents requirements, install steps, live-preview-without-reboot procedure, uninstall, and packaging pointer.
- TODO.md updated: "Verify theme renders during Kiro ISO boot" closed (confirmed working); "Write install instructions in README.md" closed; "Test with `plymouth --show-splash` in a VM" dropped from backlog (covered by the README's live-preview section).
- Breathing-glow halo finally reads on boot. The 2026-05-18 implementation was technically present but visually imperceptible — the "halo" was being generated at runtime by `Image.Scale(progress-0.png, 1.25×)`, but `progress-0.png` is opaque grayscale with no alpha channel, so the scaled "halo" was just a slightly-larger opaque black rectangle behind an opaque logo. No glow could read. Replaced runtime scaling with a pre-baked alpha-channel halo PNG.

### Technical Details
- README structured around the canonical Arch/Kiro Plymouth install: copy theme → add `plymouth` hook to `mkinitcpio.conf` → `plymouth-set-default-theme -R startrek` → `quiet splash` in `GRUB_CMDLINE_LINUX_DEFAULT`. Includes a TTY-based preview using `plymouthd --debug` + `plymouth --show-splash` so the theme can be tested without rebooting.
- Mirrors the structure used by other EDU theme repos so a user landing on either repo gets the same install pattern.
- Halo bake — two-step ImageMagick pipeline. First attempt used `-alpha copy` on the raw outline, which produced an unrecognizable blob because the delta is a thin outline, not a filled shape. Solved by flood-filling the interior first to get a solid delta silhouette, then masking and blurring that:
  ```
  # Step 1: flood-fill the delta interior to produce a solid silhouette mask
  magick progress-0.png -threshold 30% \
    -bordercolor black -border 1 \
    -fill gray50 -draw "color 0,0 floodfill" \
    -fill white +opaque gray50 \
    -fill black -opaque gray50 \
    -shave 1x1 -alpha off /tmp/mask.png

  # Step 2: turn black background into transparent, border + blur for soft halo
  magick /tmp/mask.png -alpha set -transparent black \
    -bordercolor none -border 80x80 -blur 0x14 glow.png
  ```
  - **Step 1 (mask)**: threshold to clean binary; add a 1px black border so the outer-corner flood-fill starts on a guaranteed-black pixel connected to the outside; flood the outside region with gray50; `+opaque gray50` paints everything not-gray (= original outline + enclosed interior) to white; `-opaque gray50` puts the background back to black; shave the helper border. Result: black background, fully-filled white delta + tail.
  - **Step 2 (halo)**: `-transparent black` converts the black background pixels to alpha=0 while keeping white pixels opaque white; 80px transparent border gives the blur canvas to bleed into; `-blur 0x14` (σ=14) gives ~30–40px of soft falloff around the delta silhouette.
  - Direct `-alpha copy` on the original outline produced no halo because the delta is thin lines on opaque black — copying luma to alpha gave a thin-line alpha mask, and a heavy blur of thin lines dissolves into a featureless white blob (verified during this session).
- Script change: dropped runtime `glow_scale` / `Image.Scale()` math; loads `glow.png` directly and centers it behind `logo_sprite` using the size delta. The sin-wave breathing math and opacity multipliers (0.8→1.0 logo, 0.0→0.45 glow) are unchanged.

### Files Modified
- README.md
- TODO.md
- CHANGELOG.md
- `usr/share/plymouth/themes/startrek/glow.png` (created — 18.9 KB baked asset)
- `usr/share/plymouth/themes/startrek/startrek.script` (glow layer replaced)
- CLAUDE.md (Animation pattern + Current state updated)

## 2026.05.18

### What Changed
Replaced the 24-frame spinning logo animation with a static logo that breathes/glows. The rotation (cycling through `progress-0.png` to `progress-23.png`) was removed entirely. A sin-wave driven opacity pulse and a scaled glow halo sprite were added instead.

### Technical Details
Plymouth's `Sprite.SetOpacity()` and `Image.Scale()` were used to build a two-layer glow effect:
- Main logo: `progress-0.png` at fixed position, opacity oscillating 0.8→1.0
- Glow sprite: same image scaled 1.25×, centered behind the logo via `SetZ(-1)`, opacity oscillating 0.0→0.45
- Both driven by `Math.Sin(state.time * 0.05)` — approximately a 2.5-second breathing cycle at Plymouth's 50 Hz refresh rate
- Glow sprite is pre-scaled once at startup (no per-frame cost)

### Files Modified
- `usr/share/plymouth/themes/startrek/startrek.script`
- CHANGELOG.md (created)
- CLAUDE.md (created)
- TODO.md (created)
- IDEAS.md (created)
