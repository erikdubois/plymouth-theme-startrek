# plymouth-theme-startrek

A Plymouth boot splash theme for Arch / Kiro Linux with a Star Trek Starfleet look. A static central logo with a soft breathing glow (~2.5 s sin-wave cycle at 50 Hz), built from 24 progress frames.

## Contents

- `usr/share/plymouth/themes/startrek/` — theme assets
  - `startrek.plymouth` — theme manifest
  - `startrek.script` — animation script (breathing-glow sprites)
  - `progress-0.png` … `progress-23.png` — frames

## Requirements

- Arch Linux (or Kiro). Other distros work but the steps below assume `pacman` + `mkinitcpio` + GRUB.
- `plymouth` installed:
  ```bash
  sudo pacman -S plymouth
  ```

## Install

1. **Copy the theme into place**

   From a clone of this repo:
   ```bash
   sudo cp -r usr/share/plymouth/themes/startrek /usr/share/plymouth/themes/
   ```

2. **Add the `plymouth` hook to mkinitcpio**

   Edit `/etc/mkinitcpio.conf` and add `plymouth` to `HOOKS`, immediately after `base udev`:
   ```
   HOOKS=(base udev plymouth autodetect microcode modconf kms keyboard keymap consolefont block filesystems fsck)
   ```

3. **Set as default theme and rebuild initramfs**
   ```bash
   sudo plymouth-set-default-theme -R startrek
   ```
   The `-R` flag rebuilds initramfs for every installed kernel.

4. **Enable splash on the kernel command line**

   Edit `/etc/default/grub` and make sure `GRUB_CMDLINE_LINUX_DEFAULT` contains `quiet splash`:
   ```
   GRUB_CMDLINE_LINUX_DEFAULT="quiet splash"
   ```
   Then regenerate the GRUB config:
   ```bash
   sudo grub-mkconfig -o /boot/grub/grub.cfg
   ```

5. **Reboot** — the Starfleet splash should appear during boot.

## Test without rebooting

To preview the theme on the running system:
```bash
sudo plymouthd --debug --tty=tty1
sudo plymouth --show-splash
# leave it visible for a few seconds, then:
sudo plymouth --quit
```
Run from a TTY (Ctrl+Alt+F2), not from inside your graphical session.

## Uninstall

```bash
sudo plymouth-set-default-theme -R bgrt    # or any other installed theme
sudo rm -rf /usr/share/plymouth/themes/startrek
```
Remove `plymouth` from `HOOKS` in `/etc/mkinitcpio.conf` and drop `splash` from `GRUB_CMDLINE_LINUX_DEFAULT` if you want Plymouth off entirely. Rebuild initramfs and GRUB config after editing.

## Packaging

A `nemesis_repo` PKGBUILD is on the TODO list; until then, install from this repo as above.

## License

See [LICENSE](./LICENSE).
