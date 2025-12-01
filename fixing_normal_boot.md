# Adding `dis_ucode_ldr` to GRUB Configuration

This guide focuses on the specific step of adding the `dis_ucode_ldr` parameter to the GRUB configuration.

## Step: Add `dis_ucode_ldr` Parameter

1. Open the GRUB configuration file in a text editor:

```bash
sudo nano /etc/default/grub
```

2. Locate the line starting with:

```bash
GRUB_CMDLINE_LINUX_DEFAULT="..."
```

3. **Add `dis_ucode_ldr` inside the quotes.**

**Example:**

```bash
GRUB_CMDLINE_LINUX_DEFAULT="quiet splash dis_ucode_ldr"
```

> This disables early CPU microcode loading, which can resolve certain boot issues on some ASUS laptops.

4. Save the file (`Ctrl+O` in nano) and exit (`Ctrl+X`).

5. Update GRUB and reboot:

```bash
sudo update-grub
sudo reboot
```

Your system should now boot normally.
