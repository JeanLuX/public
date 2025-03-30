# Safely remove bloatwares on Xiaomi 15 Ultra Global Version (HyperOS 2.0.9.0)

## 1. Prerequisites

### A. Enable Developer Options and USB Debugging
1. Go to **Settings** > **About phone** and tap **OS version** 7 times.
2. Go to **Settings** > **Additional settings** > **Developer options** and enable **USB debugging**.

### B. Install ADB
A useful article on XDA : https://www.xda-developers.com/install-adb-windows-macos-linux/#how-to-set-up-adb-on-your-computer

## 2. Connect Your Xiaomi 15 Ultra
1. Connect your device via USB.
2. Run `adb devices` in the terminal/command prompt and the Xiaomi 15 Ultra prompt screen, authorize your computer.
3. Open the ADB Shell with `adb shell`.

## 3. Safe Bloatware Removal Commands
Execute these commands **in the previously opened ADB Shell**:

```bash
pm uninstall -k --user 0 com.alibaba.aliexpresshd
pm uninstall -k --user 0 com.altice.android.myapps
pm uninstall -k --user 0 com.amazon.appmanager
pm uninstall -k --user 0 com.audible.application
pm uninstall -k --user 0 com.aura.oobe.deutsche
pm uninstall -k --user 0 com.aura.oobe.vodafone
pm uninstall -k --user 0 com.bsp.catchlog
pm uninstall -k --user 0 com.dti.bouyguestelecom
pm uninstall -k --user 0 com.dti.telefonica
pm uninstall -k --user 0 com.facebook.appmanager
pm uninstall -k --user 0 com.facebook.services
pm uninstall -k --user 0 com.facebook.system
pm uninstall -k --user 0 com.goodix.fingerprint.producttest
pm uninstall -k --user 0 com.google.android.apps.docs
pm uninstall -k --user 0 com.google.android.apps.maps
pm uninstall -k --user 0 com.google.android.apps.restore
pm uninstall -k --user 0 com.google.android.apps.safetyhub
pm uninstall -k --user 0 com.google.android.apps.setupwizard.searchselector
pm uninstall -k --user 0 com.google.android.apps.subscriptions.red
pm uninstall -k --user 0 com.google.android.apps.tachyon
pm uninstall -k --user 0 com.google.android.apps.wellbeing
pm uninstall -k --user 0 com.google.android.apps.youtube.music
pm uninstall -k --user 0 com.google.android.gms.location.history
pm uninstall -k --user 0 com.google.android.gms.supervision
pm uninstall -k --user 0 com.google.android.marvin.talkback
pm uninstall -k --user 0 com.google.android.projection.gearhead
pm uninstall -k --user 0 com.google.android.videos
pm uninstall -k --user 0 com.google.android.youtube
pm uninstall -k --user 0 com.ironsource.appcloud.oobe.hutchison
pm uninstall -k --user 0 com.linkedin.android
pm uninstall -k --user 0 com.mi.globalbrowser
pm uninstall -k --user 0 com.microsoft.appmanager
pm uninstall -k --user 0 com.microsoft.deviceintegrationservice
pm uninstall -k --user 0 com.microsoftsdk.crossdeviceservicebroker
pm uninstall -k --user 0 com.miui.analytics
pm uninstall -k --user 0 com.miui.audiomonitor
pm uninstall -k --user 0 com.miui.bugreport
pm uninstall -k --user 0 com.miui.cleaner
pm uninstall -k --user 0 com.miui.daemon
pm uninstall -k --user 0 com.miui.miservice
pm uninstall -k --user 0 com.miui.mishare.connectivity
pm uninstall -k --user 0 com.miui.miwallpaper.overlay
pm uninstall -k --user 0 com.miui.miwallpaper.overlay.customize
pm uninstall -k --user 0 com.miui.msa.global
pm uninstall -k --user 0 com.miui.player
pm uninstall -k --user 0 com.miui.touchassistant
pm uninstall -k --user 0 com.miui.videoplayer
pm uninstall -k --user 0 com.miui.yellowpage
pm uninstall -k --user 0 com.orange.aura.oobe
pm uninstall -k --user 0 com.orange.update
pm uninstall -k --user 0 com.qti.dcf
pm uninstall -k --user 0 com.qti.qcc
pm uninstall -k --user 0 com.qualcomm.atfwd
pm uninstall -k --user 0 com.qualcomm.atfwd2
pm uninstall -k --user 0 com.qualcomm.qti.remoteSimlockAuth
pm uninstall -k --user 0 com.sfr.android.sfrjeux
pm uninstall -k --user 0 com.tencent.soter.soterserver
pm uninstall -k --user 0 com.wdstechnology.android.kryten
pm uninstall -k --user 0 com.xiaomi.barrage
pm uninstall -k --user 0 com.xiaomi.glgm
pm uninstall -k --user 0 com.xiaomi.joyose
pm uninstall -k --user 0 com.xiaomi.mipicks
pm uninstall -k --user 0 com.xiaomi.mirror
pm uninstall -k --user 0 com.xiaomi.mtb
pm uninstall -k --user 0 com.xiaomi.payment
pm uninstall -k --user 0 com.xiaomi.simactivate.service
pm uninstall -k --user 0 com.xiaomi.xmsfkeeper
pm uninstall -k --user 0 de.telekom.tsc
pm uninstall -k --user 0 org.ifaa.aidl.manager
