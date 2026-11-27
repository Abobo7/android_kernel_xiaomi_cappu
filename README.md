# android_kernel_xiaomi_cappu

Clean-room Linux 3.18.35 kernel tree for **Xiaomi Mi Pad 3 (`cappu` / `xiaomi8176_tb_c9_n`, MediaTek MT8176 + PowerVR GX6250)** targeting **LineageOS 17.1 (Android 10)**.

## Branches
- **`cappu-n-oss`**: Verbatim official baseline from [`MiCode/Xiaomi_Kernel_OpenSource`](https://github.com/MiCode/Xiaomi_Kernel_OpenSource/tree/cappu-n-oss) (`59c67c586f44`).
- **`lineage-17.1`**: Clean-room Android 10 kernel branch backported directly from Google AOSP [`kernel/common` (`deprecated/android-3.18`)](https://android.googlesource.com/kernel/common/+/refs/heads/deprecated/android-3.18):
  - Multi-domain Scatter-Gather Binder (`binder,hwbinder,vndbinder` + `binder_alloc` + `mmput_async`)
  - Ambient Capabilities (`PR_CAP_AMBIENT = 47`)
  - TCP + UDP `SOCK_DESTROY` (`CONFIG_INET_DIAG_DESTROY=y`, `CONFIG_INET_UDP_DIAG=y`)
  - AOSP `configfs` `sdcardfs` (`/config/sdcardfs`, `default_normal`, `unshared_obb`)
  - Per-UID CPU/IO & `syscfs` stats (`CONFIG_UID_SYS_STATS=y`)
  - AOSP `kernel/bpf/` verifier fix & Android 10 netfilter/cgroup/quota configs (`lineageos_cappu_defconfig`)

## Standalone Build
```bash
export ARCH=arm64
export TC="$HOME/mipad3-bringup/toolchains/aarch64-linux-android-4.9/bin/aarch64-linux-android-"
make O=out ARCH=arm64 CROSS_COMPILE="$TC" lineageos_cappu_defconfig
make -j$(nproc) O=out ARCH=arm64 CROSS_COMPILE="$TC" CC="ccache ${TC}gcc"
# Output: out/arch/arm64/boot/Image.gz-dtb
```
