# Ambient Light Sensor Firmware on Samsung Galaxy Book6 Ultra

This procedure installs Samsung's Panther Lake Integrated Sensor Hub (ISH)
firmware so Linux can detect the ambient light sensor.

## Scope

Use only on:

| Item | Expected value |
|---|---|
| Model | Samsung Galaxy Book6 Ultra, `NP960UJG-KG2FR` |
| ISH controller | Intel Panther Lake, PCI `8086:e445` |
| PCI subsystem | Samsung `144d:ca0a` |

The generic firmware fails with:

```text
ISH loader: cmd 2 failed 10
```

Do not use firmware from another computer, manufacturer, or Intel generation.

## 1. Download

Open the [Samsung Galaxy Books Download Center](https://www.samsung.com/global/galaxybooks-downloadcenter/model/?modelCode=NP960UJG-KG2FR&siteCode=fr).

1. Select **Sensor**.
2. Select **Intel Sensor Hub**.
3. Download the latest **Motion sensor driver**.

The archive name can change. Example:

```text
BASW-A3934A1A_1063.zip
```

Required tools:

```text
unzip install find python3 lspci journalctl
```

## 2. Check Hardware

```bash
lspci -v -nn -s 00:12.0
```

Continue only if the output contains:

```text
Intel Corporation ... ISH [8086:e445]
Subsystem: Samsung ... [144d:ca0a]
Kernel driver in use: intel_ish_ipc
```

## 3. Extract OEM Firmware

Put the downloaded ZIP in the current directory, then run:

```bash
ZIP=$(find . -maxdepth 1 -type f -iname '*.zip' -print -quit)
WORK_DIR="${TMPDIR:-/tmp}"
DRIVER_DIR="$WORK_DIR/samsung-ish-driver"
OUT="$WORK_DIR/samsung-np960ujg-ish-firmware"

unzip -tq "$ZIP"
mkdir -p "$DRIVER_DIR"
unzip -oq "$ZIP" -d "$DRIVER_DIR"
```

Select the Samsung OEM Panther Lake image. Do **not** select the generic Intel
image from `IshHeci`:

```bash
OEM_FW=$(find \
  "$DRIVER_DIR/IshHeciExtensionTemplate/x64/FwImage/0004" \
  -maxdepth 1 -type f -name 'PTL_*.bin' -print -quit)

test -f "$OEM_FW"
stat -c '%n: %s bytes' "$OEM_FW"
```

The path must start with:

```text
IshHeciExtensionTemplate/x64/FwImage/0004/PTL_
```

## 4. Generate Linux Filename

Linux selects OEM firmware using CRC32 values from the current DMI fields. The
following computes the name automatically:

```bash
crc32_dmi() {
  python3 -c 'import sys, zlib; print(f"{zlib.crc32(sys.argv[1].encode()):08x}")' "$1"
}

SYS_VENDOR=$(< /sys/class/dmi/id/sys_vendor)
PRODUCT_NAME=$(< /sys/class/dmi/id/product_name)
PRODUCT_SKU=$(< /sys/class/dmi/id/product_sku)

LINUX_FW="ish_ptl_$(crc32_dmi "$SYS_VENDOR")_$(crc32_dmi "$PRODUCT_NAME")_$(crc32_dmi "$PRODUCT_SKU").bin"

install -d -m 0755 "$OUT"
install -m 0644 "$OEM_FW" "$OUT/$LINUX_FW"
printf 'Prepared: %s\n' "$OUT/$LINUX_FW"
```

## 5. Install the Firmware

### Ubuntu 26.04 LTS

Ubuntu uses the standard firmware path. Install the file and rebuild the
initramfs:

```bash
sudo install -d -m 0755 /usr/lib/firmware/intel/ish
sudo cp "$OUT/$LINUX_FW" /usr/lib/firmware/intel/ish/
sudo update-initramfs -u -k all
```

If the system uses Dracut instead of `initramfs-tools`, use:

```bash
sudo dracut --regenerate-all --force
```

### Fedora Non-Atomic

On Fedora Workstation, Server, or another mutable Fedora edition:

```bash
sudo install -d -m 0755 /usr/lib/firmware/intel/ish
sudo cp "$OUT/$LINUX_FW" /usr/lib/firmware/intel/ish/
sudo restorecon -RF /usr/lib/firmware/intel/ish
sudo dracut --regenerate-all --force
```

### Fedora Atomic

On Fedora Atomic distributions such as Aurora, Kinoite, or Silverblue, `/usr`
is immutable. Install the firmware under `/etc`, which is persistent and
included in the initramfs overlay:

```bash
sudo install -d -m 0755 /etc/firmware/intel/ish
sudo cp "$OUT/$LINUX_FW" /etc/firmware/intel/ish/
sudo restorecon -RF /etc/firmware
```

The `intel_ish_ipc` driver is probed very early during boot (before about
1.7 seconds), before `switch_root` changes to the real root filesystem. A file
placed only in a persistent filesystem path is therefore not found
because that path still refers to the initramfs environment at that point.

Tell the kernel to search this firmware path:

```bash
sudo rpm-ostree kargs \
  --append-if-missing='firmware_class.path=/etc/firmware'
```

Explicitly inject the file into the initramfs with `dracut -I`:

```bash
sudo rpm-ostree initramfs --enable --arg=-I \
  --arg=/etc/firmware/intel/ish/$LINUX_FW
```

The two commands above each create a new deployment. Reboot only once, after
both deployments are ready.

## 6. Restart and Verify

The ISH loader needs a full platform reset after rejecting firmware:

```bash
sudo systemctl poweroff
```

Start the computer manually, then check:

```bash
journalctl -k -b | grep -iE 'intel_ish_ipc|ISH loader|ish-hid|sensor hub'
```

Expected: `firmware loaded`, no `cmd 2 failed 10`, and ISH HID devices
enumerated. The kernel log should show:

```text
load firmware: intel/ish/$LINUX_FW
```

with the customized filename, not the generic `intel/ish/ish_ptl.bin`.

If loading fails, check the chronological order of ISH initialization and the
root switch:

```bash
sudo journalctl -k -b | grep -iE 'ISH loader|switch.?root'
```

Check the ambient light sensor:

```bash
find /sys/bus/iio/devices -maxdepth 2 -type f -name name -print -exec cat {} \;
monitor-sensor
```

Expected:

```text
Has ambient light sensor
```

The lux value should change when the sensor is covered or exposed to light.

In KDE (Plasma), **Display Configuration** should now show the
**Automatically adapt to environment** option.

## License

The blob comes from Samsung's Windows driver and is not known to have a Linux
redistribution license. Keep it for local use unless Samsung grants permission
to redistribute it.
