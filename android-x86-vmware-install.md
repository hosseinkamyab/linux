
# Installing Android-x86 9.0 on VMware — Full Guide & Troubleshooting Notes

This document explains the full process of installing **Android-x86 9.0** on a VMware virtual machine when the live session cannot boot. It also covers how to fix the boot issue using `nomodeset` and how to permanently update GRUB so Android boots successfully every time.

---

## 1. Downloading Android-x86

1. Visit the official website:
   **[https://www.android-x86.org](https://www.android-x86.org)**
2. Download the **Android-x86 9.0 ISO**.
3. Create a new VMware virtual machine:

   * **Guest OS:** Other Linux (64-bit)
   * **Disk:** 8–16 GB
   * **RAM:** 2–4 GB
   * **CPU:** 2 cores recommended
   * (Optional) Enable **EFI**

---

## 2. Live Mode Fails to Boot

When selecting **“Run Live CD – Android-x86 9.0”**, the VM may:

* freeze
* show a blank screen
* fail to load Android

This happens due to graphics compatibility issues with VMware.

---

## 3. Automatic Installation of Android-x86

1. Boot the VM from the ISO.
2. In the boot menu, go to:

```
Advanced options → Auto Install to specified harddisk
```

3. The installer automatically partitions the disk and installs Android.
4. When installation finishes, you are given a **Run** option —
   **ignore it**, because it usually doesn’t work.

**Select Reboot instead.**

---

## 4. First Boot Failure (Temporary Fix)

After rebooting, Android still won’t start normally.

To fix temporarily:

1. At the GRUB menu, highlight the first boot option.
2. Press **e** to edit the boot entry.
3. Find the line starting with:

```
kernel ...
```

4. Append the parameter:

```
nomodeset
```

5. Press **B** to boot.

Android-x86 should now load (possibly slowly).

---

## 5. Making the Fix Permanent (Editing GRUB)

To avoid manually typing `nomodeset` every time, update GRUB inside Android.

### Step 1 — Open the Console

After Android boots, press:

```
Alt + F1
```

This opens a terminal-like interface (TTY).

---

### Step 2 — Mount the System Partition

Create a mount directory:

```bash
mkdir /mnt/sda
```

Mount the Android system partition:

```bash
mount /dev/block/sda1 /mnt/sda
```

> Note: If auto-install created multiple partitions, the correct one might be `sda2`, but `sda1` is common.

---

### Step 3 — Edit `menu.lst`

Open the GRUB configuration file:

```bash
vi /mnt/sda/grub/menu.lst
```

Inside the file:

* Optionally set GRUB timeout to **0** so it boots instantly:

```
timeout 0
```

* Locate the first boot entry and append `nomodeset` to the kernel line, e.g.:

```
kernel /android-9.0/kernel root=/dev/ram0 androidboot.hardware=android_x86 nomodeset
```

---

### Step 4 — Save Changes

In `vi`:

1. Press **i** to begin editing.
2. Make the necessary changes.
3. Press **Esc**.
4. Type:

```
:wq
```

This writes the changes and exits.

---

### Step 5 — Reboot

```bash
reboot
```

Your Android-x86 VM should now boot successfully **every time**, without editing GRUB manually.

---

## 6. Summary

* Live mode doesn’t boot in VMware → use **Automatic Install**.
* Android fails to boot after install → add **nomodeset**.
* Make the fix permanent → edit `/mnt/sda/grub/menu.lst`.
* System now loads normally without further manual steps.

