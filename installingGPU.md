# NVIDIA GPU and PRIME Installation on Debian 13 Trixie (ASUS N552VS)

This document summarizes the steps to install and configure NVIDIA drivers with PRIME offload on a hybrid Intel + NVIDIA laptop running Debian 13 "Trixie".

---

## 1. Configure Repositories

Ensure `/etc/apt/sources.list` includes `main contrib non-free non-free-firmware`:

```text
deb http://deb.debian.org/debian/ trixie main contrib non-free non-free-firmware
deb http://deb.debian.org/debian/ trixie-updates main contrib non-free non-free-firmware
deb http://security.debian.org/debian-security trixie-security main contrib non-free non-free-firmware
```

Update package lists:

```bash
sudo apt update
```

---

## 2. Install Build Tools and Kernel Headers

Install prerequisites for building the NVIDIA kernel module:

```bash
sudo apt install build-essential dkms linux-headers-$(uname -r) xserver-xorg-video-intel -y
```

* `build-essential` → compiler tools
* `dkms` → automatic module rebuild on kernel updates
* `linux-headers-$(uname -r)` → headers for current kernel
* `xserver-xorg-video-intel` → keeps Intel GPU working

---

## 3. Blacklist Nouveau

Prevent conflicts with the open-source driver:

```bash
sudo tee /etc/modprobe.d/blacklist-nouveau.conf <<EOF
blacklist nouveau
options nouveau modeset=0
EOF
```

Rebuild initramfs:

```bash
sudo update-initramfs -u
```

---

## 4. Enable NVIDIA DRM Modeset

Required for PRIME offload:

```bash
sudo tee /etc/modprobe.d/nvidia.conf <<EOF
options nvidia-drm modeset=1
EOF
```

---

## 5. Install NVIDIA Driver

Install the proprietary driver and GLVND libraries:

```bash
sudo apt install --reinstall nvidia-driver nvidia-kernel-dkms libglvnd-dev libglvnd0 libgl1-nvidia-glvnd-glx
```

---


## 6. Reboot

```bash
sudo reboot
```

---

## 7. Verification Commands

### Check NVIDIA Kernel Modules

```bash
lsmod | grep nvidia
```

### Test NVIDIA Driver

```bash
nvidia-smi
```

### Check Default GPU

```bash
glxinfo | grep "OpenGL renderer"
```

* Should show Intel HD Graphics 530

### Test PRIME Offload

```bash
__NV_PRIME_RENDER_OFFLOAD=1 __GLX_VENDOR_LIBRARY_NAME=nvidia glxinfo | grep "OpenGL renderer"
```

* Should show NVIDIA GeForce GTX 950M

### Test GPU with a Sample App

```bash
__NV_PRIME_RENDER_OFFLOAD=1 __GLX_VENDOR_LIBRARY_NAME=nvidia glxgears
```

* High FPS → NVIDIA GPU is rendering

### Check Installed NVIDIA Packages

```bash
dpkg -l | grep nvidia
```

---

## ✅ Summary

* Build tools + DKMS + headers → allow NVIDIA kernel module compilation
* Blacklist Nouveau + enable DRM modeset → prevent conflicts
* Install NVIDIA driver properly → kernel module + GL libraries
* Verification ensures NVIDIA GPU and PRIME offload work correctly
