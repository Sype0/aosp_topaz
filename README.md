# AOSP 16 for Redmi Note 12 4G (topaz / tapas)

Plain AOSP **android-16.0.0_r4** (Android 16 QPR2, release config `bp4a`),
built on GitHub Actions. No vendor/lineage, no GApps, test keys.

Why this version: it is the AOSP tag LineageOS 23.2 is based on, and the
topaz device, vendor and sepolicy trees already build and boot on it. Android 13
(the shipping version) would need the trees backported; Android 17 trees here
are crDroid-only. A plain AOSP 16 system is also the lightest system these
trees run.

## What goes into the tree

- `repo init` of AOSP `android-16.0.0_r4` + [`local_manifests.xml`](local_manifests.xml)
- Device tree `Sype0/android_device_xiaomi_topaz` branch `aosp-16` (product `aosp_topaz`).
  Its `aosp/` directory holds the QCOM build glue LineageOS keeps in vendor/lineage
  and build/make.
- Kernel: BaranAspect's prebuilt (`lineage-23.0`) via `Sype0/android_kernel_xiaomi_topaz`
  branch `aosp-16`, which only adds the kernel header modules AOSP lacks.
- CAF/QCOM hardware projects (HAL sources only, the system stays AOSP).

## Build

Actions → *Build AOSP 16 (topaz)* → Run workflow. A build that runs out of the
6 h job limit saves `out/` to the `ci-out-state` release and starts the next
run, which carries on (`chain_left`). On a build error the job waits for a fix
pushed to this repo or any project in the local manifest (`wait_for_fix`); the
error is on the `ci-status` branch, live progress on `ci-live`.

The finished ROM is a release: `AOSP-16-topaz-<date>-<variant>.zip` (flash from
recovery with `adb sideload`) plus boot, init_boot, vendor_boot, dtbo, recovery
and vbmeta images.

Locally: `lunch aosp_topaz-bp4a-userdebug && m otapackage`.
