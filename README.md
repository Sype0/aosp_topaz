# AOSP 17 for Redmi Note 12 4G (topaz / tapas)

Plain AOSP **android-17.0.0_r1** (Android 17, release config `cp2a`),
built on GitHub Actions. No vendor/lineage, no GApps, test keys.

The Android 16 (android-16.0.0_r4) version of this build is on the
[`aosp-16`](../../tree/aosp-16) branch.

## What goes into the tree

- `repo init` of AOSP `android-17.0.0_r1` + [`local_manifests.xml`](local_manifests.xml)
- Device tree `Sype0/android_device_xiaomi_topaz` branch `aosp-17` (product `aosp_topaz`):
  the crDroid 17 tree without vendor/lineage. Its `aosp/` directory holds the
  QCOM build glue LineageOS keeps in vendor/lineage and build/make.
- Kernel: BaranAspect's prebuilt (`lineage-23.0`) via `Sype0/android_kernel_xiaomi_topaz`
  branch `grapheneos-17`, which only adds the kernel header modules AOSP lacks.
- CAF/QCOM hardware projects on LineageOS 24.0 branches (HAL sources only, the
  system stays AOSP). Where a project needed a change to build without
  vendor/lineage, the Sype0 fork's `grapheneos-17` branch has it; those
  branches carry no GrapheneOS code and are shared with
  [grapheneos_topaz](https://github.com/Sype0/grapheneos_topaz).

## Build

Actions → *Build AOSP 17 (topaz)* → Run workflow. A build that runs out of the
6 h job limit saves `out/` to the `ci-out-state-17` release and starts the next
run, which carries on (`chain_left`). On a build error the job waits for a fix
pushed to this repo or any project in the local manifest (`wait_for_fix`); the
error is on the `ci-status` branch, live progress on `ci-live`.

The finished ROM is a release: `AOSP-17-topaz-<date>-<variant>.zip` (flash from
recovery with `adb sideload` after a format data) plus boot, init_boot,
vendor_boot, dtbo, recovery and vbmeta images.

**Never relock the bootloader.** A locked Xiaomi bootloader only boots
Xiaomi-signed images.

Locally: `lunch aosp_topaz-cp2a-userdebug && m otapackage`.
