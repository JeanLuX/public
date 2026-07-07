# Samsung Galaxy Book6 Ultra (NP960UJG-KG2FR) on Linux

## Hardware

- Model: Samsung Galaxy Book6 Ultra (NP960UJG-KG2FR | PVAL)
- CPU: Intel Core Ultra X7 358H
- iGPU: Intel Arc B390
- RAM: 32 GB
- Storage: 1 TB NVMe SSD

## Tested Software Environment

- Operating System: Aurora 44 (based on Fedora 44 Kinoite)
- Kernel Version: 7.1.4-202.fc44.x86_64 (64-bit)
- Graphics Platform: Wayland

## Current Issues

- Without the required Cirrus Logic firmware, audio works only in a minimal/fallback mode. The firmware can be extracted from Samsung's official Windows driver and installed on Linux, significantly improving audio quality and bringing it close to the Windows experience without Dolby processing or other artificial enhancements. That is more than sufficient for everyday use and restores the built-in speakers' intended character and capability.
  - Extraction procedure: [AUDIO-CIRRUS-FIRMWARE-EXTRACTION.md](./AUDIO-CIRRUS-FIRMWARE-EXTRACTION.md)
  - Related issue: https://github.com/antoinecellerier/speaker-tuning-to-easyeffects/issues/27
- If Adaptive Sync is enabled on the internal display, you should use the kernel parameter `xe.enable_panel_replay=0`. Without it, the internal screen may show artifacts, become unusable intermittently, or even trigger a full system crash under internal display load.
  - Note: enabling this parameter seems to slightly reduce battery life.
  - If battery life is the priority, disabling Adaptive Sync entirely may be preferable. Nonetheless, this does not prevent very occasional artifacts on the internal screen, especially during multiple sleep/wake cycles with Electron applications.
- Forcing active sync mode instead of automatic mode feels sluggish (persistent low-FPS behavior, less responsive cursor/touchpad feel).
- HDR is supported by the internal Samsung OLED panel, but KDE Plasma currently reports it as unavailable. The panel EDID advertises DisplayID 2.0 with an encapsulated CTA-861 block containing BT.2020 RGB and SMPTE ST 2084/PQ metadata (approximately 1100 nits peak brightness). The installed `libdisplay-info 0.3.0` does not search CTA blocks nested in DisplayID 2.0, so KWin receives neither HDR nor wide-gamut capability. The upstream parser fix is included from `libdisplay-info 0.4.0`; Aurora/Fedora 44 still provides `0.3.0` and needs an update or backport.
  - Local status: `kscreen-doctor` reports `HDR: incapable` and `Wide Color Gamut: incapable` for `eDP-1`.
  - Workaround: forcing HDR support by adding `KWIN_FORCE_ASSUME_HDR_SUPPORT=1` to `/etc/environment` makes the HDR option appear in KDE Display Configuration.
    - Note: The HDR content is displayed correctly, but SDR content is dimmer than expected, likely under Windows 11 with HDR enabled.
  - References: [KWin HDR EDID override](https://invent.kde.org/plasma/kwin/-/merge_requests/7337), [libdisplay-info nested CTA fix](https://chromium.googlesource.com/external/gitlab.freedesktop.org/emersion/libdisplay-info/+/73ec53d800275cce4b8a4af6af03ea2aac9912fd), [Samsung panel measurements](https://www.notebookcheck.com/Samsung-Galaxy-Book6-Ultra-im-Test-Beeindruckender-Multimedia-Laptop-mit-tollem-OLED-und-RTX-5070.1243968.0.html)
- Ambient light sensor is not detected by KDE, so automatic brightness adjustment (as on Windows 11) is unavailable.
    ```
    # kscreen-doctor --outputs
    Output: 1 eDP-1 <uuid>
    enabled
    connected
    priority 1
    Panel
    replication source:0
    Modes:  1:2880x1800@120.00*!  2:2880x1800@60.00  3:1600x1200@59.87  4:1600x1200@119.82  5:1280x1024@59.90  6:1280x1024@119.83  7:1024x768@59.92  8:1024x768@119.80  9:2560x1600@59.99  10:2560x1600@119.93  11:1920x1200@59.88  12:1920x1200@119.90  13:1280x800@59.81  14:1280x800@119.85  15:2880x1620@59.96  16:2880x1620@119.95  17:2560x1440@59.96  18:2560x1440@119.95  19:1920x1080@59.96  20:1920x1080@119.93  21:1600x900@59.95  22:1600x900@119.95  23:1368x768@59.88  24:1368x768@119.83  25:1280x720@59.85  26:1280x720@119.86
    Custom modes: None
    Geometry: 0,0 1920x1200
    Scale: 1.5
    Rotation: 1
    Overscan: 0
    Vrr: Never
    RgbRange: Automatic
    HDR: incapable
    Wide Color Gamut: incapable
    ICC profile: none
    Color profile source: sRGB
    Color power preference: prefer efficiency and performance
    Brightness control: supported, set to 100% and dimming to 100%
    Color resolution: automatic (10), range: [6; 12] bits per color
    Allow EDR: never
    Sharpness control: supported, set to 0%
    Automatic brightness: unsupported
    Auto Rotate Policy: incapable
    Adaptive backlight modulation: unsupported
    ```
- Enabling LUKS encryption limits NVMe throughput to about 4 GB/s instead of the ~7 GB/s that the bundled SSD can achieve unencrypted. In daily usage, this is generally not very noticeable.
  - The SSD appears to support OPAL. If configured correctly, encryption can be offloaded to the SSD controller, allowing near-native performance.
  - On Windows 11, performance appears to remain near maximum even with BitLocker enabled.
- Fingerprint reader does not work, and there is no visible KDE integration. However, it can be made to work by patching fprintd (for example: https://github.com/TenSeventy7/libfprint-egismoc-sdcp/issues/9)
- Webcam is not recognized yet. There is still good hope, since a project has managed to make it work. It may only require an additional device ID declaration in the kernel/driver.
  - Related issue: https://github.com/intel/ipu7-drivers/issues/73#issuecomment-4881775585
- The `quiet` platform profile is missing. Only `low-power` (Silent), `balanced` (Optimized), and `performance` (High performance) are exposed via `/sys/firmware/acpi/platform_profile_choices`, whereas the Quiet profile is available on Windows 11. Fans seem to not activate (or very late) outside of `performance` mode, causing significant heat buildup during heavy workloads across the entire chassis. On Windows 11, fans activate more frequently under the same usage conditions.
  ```
  # cat /sys/firmware/acpi/platform_profile_choices
  low-power balanced performance
  ```

- The following Fn shortcuts do not work: Fn+F1 (Settings), Fn+F10 (Microphone mute toggle), Fn+F11 (Camera toggle).
  - `dmesg` shows unknown ACPI notification events.
  - Related issue: https://github.com/joshuagrisham/samsung-galaxybook-extras/issues/108

## What Works Well

- Battery life is good and likely comparable to Windows 11 in practical use (no strict side-by-side benchmark performed).
  - More than 15 hours is achievable for software development workloads at 20% brightness using the Tuned Power Save power profile.
  - With a lighter, more optimized IDE than Visual Studio Code, it is likely possible to exceed 17 hours in similar conditions.
- The rest of the hardware works correctly overall.
- Sleep mode (`s2idle`) has relatively low drain (~0.38% per hour), although it is still not at MacBook-level efficiency.
