# Yocto RPi4 Weston14

Custom Yocto-based Linux image for Raspberry Pi 4 with Weston 14.
# Yocto RPi4 Weston Project

##  Overview

This project demonstrates building a **custom embedded Linux image** for Raspberry Pi 4 using the Yocto Project, with **Wayland and Weston (v10)** as the graphics stack.

The goal is to understand:

* Yocto build system
* Layer architecture
* BSP integration
* Wayland/Weston graphics pipeline

---

##  Project Structure

```
yocto-rpi4-weston/
├── sources/
│   ├── poky
│   ├── meta-raspberrypi
│   ├── meta-openembedded
├── build/
├── docs/
└── README.md
```

> Note: `sources/` and `build/` are ignored from Git (large Yocto components)

---

## Yocto Configuration

###  Yocto Version

* **Poky (Kirkstone LTS)**

###  Layers Used

* `meta`
* `meta-poky`
* `meta-yocto-bsp`
* `meta-raspberrypi`
* `meta-openembedded` (meta-oe, meta-python, meta-networking)

---

##  Target Hardware

* Raspberry Pi 4
* ARM Cortex-A72
* GPU: VideoCore VI (VC4 DRM/KMS)

---

##  Features Enabled

### Distro Features

```
wayland
opengl
```

### Image Packages

```
weston
weston-init
```

---

## 🛠️ Build Steps

### 1. Clone Poky

```
git clone -b kirkstone git://git.yoctoproject.org/poky
```

### 2. Clone Required Layers

```
git clone -b kirkstone https://github.com/agherzan/meta-raspberrypi
git clone -b kirkstone https://github.com/openembedded/meta-openembedded
```

### 3. Initialize Build Environment

```
source poky/oe-init-build-env build
```

### 4. Add Layers

```
bitbake-layers add-layer ../sources/meta-raspberrypi
bitbake-layers add-layer ../sources/meta-openembedded/meta-oe
bitbake-layers add-layer ../sources/meta-openembedded/meta-python
bitbake-layers add-layer ../sources/meta-openembedded/meta-networking
```

### 5. Configure Build

Edit `conf/local.conf`:

```
MACHINE = "raspberrypi4"

DISTRO_FEATURES:append = " wayland opengl"

IMAGE_INSTALL:append = " weston weston-init"

IMAGE_FSTYPES += " rpi-sdimg"
```

---

##  Build Image

```
bitbake core-image-minimal
```

---

---

## Runtime

On boot:

* Linux kernel initializes
* Weston compositor starts
* Wayland display is initialized

---

## Key Learnings

* Yocto layer compatibility (kirkstone alignment)
* BitBake workflow and recipes
* Difference between `DISTRO_FEATURES` and `IMAGE_INSTALL`
* BSP role (`meta-raspberrypi`)
* Wayland + Weston integration
* Image generation (`.wic`, `.rpi-sdimg`)

---

##  Issues Faced & Fixes

### Locale Error

```
locale.Error: unsupported locale setting
```

✔ Fixed by setting UTF-8 locale

---

### Layer Compatibility Error

```
Layer not compatible with kirkstone
```

✔ Fixed by checking out correct branch for all layers

---

### Git Repository Cleanup

* Removed Yocto layers from tracking
* Added `.gitignore` for build artifacts

---

##  Future Work

* Integrate Qt6 Wayland applications
* Add custom Yocto layer (`meta-custom`)
* Apply Weston patches (v8 / v14 experimentation)
* Explore DRM/KMS tuning
* Performance optimization

---

## 📌 Summary

This project demonstrates building a **production-style embedded Linux system** using Yocto with a modern Wayland-based graphics stack.

---

## 👨‍💻 Author

Siva Prasad K

