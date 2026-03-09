# LineageOS 17.1 for Samsung Galaxy S8 (SM-G950U)

Unofficial LineageOS 17.1 (Android 10) build for the Samsung Galaxy S8 US Snapdragon variant.

> **This repo tracks the working build, patches, and releases.**

---

## Device Specs

| Feature | Spec |
|---|---|
| **Model** | Samsung Galaxy S8 (SM-G950U) |
| **SoC** | Qualcomm Snapdragon 835 (MSM8998), Octa-core 2.45GHz |
| **RAM** | 4GB LPDDR4 |
| **Storage** | 64GB UFS 2.1 |
| **Display** | 5.8" Super AMOLED, 2960×1440 (WQHD+), 570 ppi |
| **Rear Camera** | 12MP, f/1.7, OIS, 4K video |
| **Front Camera** | 8MP, f/1.7 |
| **Battery** | 3000mAh (non-removable) |
| **Bluetooth** | BCM4361B0 (reports as BCM4347B0 via HCI) |
| **Wi-Fi** | 802.11 a/b/g/n/ac, dual-band |
| **NFC** | Yes |
| **Fingerprint** | Rear-mounted |

> ⚠️ **SM-G950U only (Snapdragon, US unlocked).** Not compatible with SM-G950F/N/W8 (Exynos).

---

## Status

| Feature | Status |
|---|---|
| Boot | ✅ Working |
| Display / Touch | ✅ Working |
| Wi-Fi | ✅ Working |
| Bluetooth | ✅ Working |
| Cellular / LTE | ✅ Working |
| Calls / SMS | ✅ Working |
| Cellular Data | ⚠️ Needs APN setup |
| Camera | ⚠️ Basic (HAL3 issues) |
| Fingerprint | ❌ Not working |
| NFC | ❌ Not tested |
| Tap to Wake | ❌ Not working |
| VoLTE | ❌ Not tested |

---

## Key Fixes

### Bluetooth (BCM4361B0)
The stock device tree incorrectly set `BOARD_HAVE_BLUETOOTH_QCOM`. Fixed to use the Broadcom vendor HAL:
- `BOARD_HAVE_BLUETOOTH_BCM := true` + `BOARD_HAVE_SAMSUNG_BLUETOOTH := true`
- Custom `vnd_dreamqltechn.txt` with correct UART path (`/dev/ttyHS0`)
- Firmware symlink: `BCM4347B0.hcd → bcm4361B0_murata.hcd` (chip reports as BCM4347B0 via HCI)
- **`-DLPM_SLEEP_MODE=0`** — disables LPM to fix Samsung bluesleep `uport=null` kernel crash

See [`patches/0001-libbt-vendor-dreamqltechn.patch`](patches/0001-libbt-vendor-dreamqltechn.patch)

### Cellular / LTE (libril Samsung AN10 compat)
Samsung's AN10 `libsec-ril.so` sends registration state responses in a format incompatible with AOSP `libril.so`:
- NULL response during modem init treated as NOT_REGISTERED (not an error)
- Extended struct sizes accepted with `< sizeof` instead of `!= sizeof`
- char** string format detection for Samsung version=15 quirk

See [`patches/0002-libril-samsung-an10.patch`](patches/0002-libril-samsung-an10.patch)

### Wi-Fi (BCM4361B0)
- LineageOS-built `wpa_supplicant` (Samsung binary breaks HIDL castFrom)
- CLM blob symlink: `/system/etc/wifi → /vendor/etc/wifi`
- Firmware paths set via `macloader.rc` at post-fs-data

---

## Flash Instructions

1. Boot to TWRP recovery
2. **Wipe → Format Data** (type `yes`)
3. Flash the ROM zip
4. Flash [MindTheGapps Android 10 arm64](https://mindthegapps.com) (optional)
5. Flash [Magisk](https://github.com/topjohnwu/Magisk/releases) (optional, for root)
6. Reboot System

---

## Build Info

- **LineageOS version:** 17.1 (Android 10)
- **Kernel:** Samsung NH base (msm8998)
- **Build date:** See release tag

---

## Releases

See [Releases](https://github.com/codyalank/lineage-17.1-dreamqlte/releases) for flashable zips.
