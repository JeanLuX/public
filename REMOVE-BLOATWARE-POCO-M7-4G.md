# Safely remove bloatwares on Poco M7 European Version (HyperOS 3)

## 1. Prerequisites

### A. Enable Developer Options and USB Debugging
1. Go to **Settings** > **About phone** and tap **OS version** 7 times.
2. Go to **Settings** > **Additional settings** > **Developer options** and enable **USB debugging**.

### B. Install ADB
A useful article on XDA : https://www.xda-developers.com/install-adb-windows-macos-linux/#how-to-set-up-adb-on-your-computer

## 2. Connect your Poco M7
1. Connect your device via USB.
2. Run `adb devices` in the terminal/command prompt and the Poco M7 prompt screen, authorize your computer.
3. Open the ADB Shell with `adb shell`.

## 3. Safe Bloatware Removal Commands
Execute these commands **in the previously opened ADB Shell**:

```bash
pm uninstall -k --user 0 cn.wps.moffice_eng
pm uninstall -k --user 0 cn.wps.xiaomi.abroad.lite
pm uninstall -k --user 0 com.amazon.appmanager
pm uninstall -k --user 0 com.amazon.mp3
pm uninstall -k --user 0 com.amazon.mShop.android.shopping
pm uninstall -k --user 0 com.block.juggle
pm uninstall -k --user 0 com.block.puzzle.game.hippo.mi
pm uninstall -k --user 0 com.booking
pm uninstall -k --user 0 com.einnovation.temu
pm uninstall -k --user 0 com.facebook.appmanager
pm uninstall -k --user 0 com.facebook.katana
pm uninstall -k --user 0 com.facebook.services
pm uninstall -k --user 0 com.facebook.system
pm uninstall -k --user 0 com.google.android.apps.chromecast.app
pm uninstall -k --user 0 com.google.android.apps.docs
pm uninstall -k --user 0 com.google.android.apps.magazines
pm uninstall -k --user 0 com.google.android.apps.maps
pm uninstall -k --user 0 com.google.android.apps.nbu.files
pm uninstall -k --user 0 com.google.android.apps.restore
pm uninstall -k --user 0 com.google.android.apps.safetyhub
pm uninstall -k --user 0 com.google.android.apps.setupwizard.searchselector
pm uninstall -k --user 0 com.google.android.apps.subscriptions.red
pm uninstall -k --user 0 com.google.android.apps.walletnfcrel
pm uninstall -k --user 0 com.google.android.apps.wellbeing
pm uninstall -k --user 0 com.google.android.apps.youtube.music
pm uninstall -k --user 0 com.google.android.gms.location.history
pm uninstall -k --user 0 com.google.android.gms.supervision
pm uninstall -k --user 0 com.google.android.marvin.talkback
pm uninstall -k --user 0 com.google.android.projection.gearhead
pm uninstall -k --user 0 com.google.android.videos
pm uninstall -k --user 0 com.google.android.youtube
pm uninstall -k --user 0 com.linkedin.android
pm uninstall -k --user 0 com.mi.global.shop
pm uninstall -k --user 0 com.mi.globalbrowser
pm uninstall -k --user 0 com.microsoft.skydrive
pm uninstall -k --user 0 com.microsoftsdk.crossdeviceservicebroker
pm uninstall -k --user 0 com.mintgames.triplecrush.tile.fun
pm uninstall -k --user 0 com.mintgames.wordtrip
pm uninstall -k --user 0 com.miui.analytics
pm uninstall -k --user 0 com.miui.android.fashiongallery
pm uninstall -k --user 0 com.miui.audiomonitor
pm uninstall -k --user 0 com.miui.bugreport
pm uninstall -k --user 0 com.miui.cleaner
pm uninstall -k --user 0 com.miui.daemon
pm uninstall -k --user 0 com.miui.global.packageinstaller
pm uninstall -k --user 0 com.miui.huanji
pm uninstall -k --user 0 com.miui.miservice
pm uninstall -k --user 0 com.miui.mishare.connectivity
pm uninstall -k --user 0 com.miui.miwallpaper.overlay
pm uninstall -k --user 0 com.miui.miwallpaper.overlay.customize
pm uninstall -k --user 0 com.miui.msa.global
pm uninstall -k --user 0 com.miui.player
pm uninstall -k --user 0 com.miui.videoplayer
pm uninstall -k --user 0 com.miui.yellowpage
pm uninstall -k --user 0 com.netflix.mediaclient
pm uninstall -k --user 0 com.opera.app.news
pm uninstall -k --user 0 com.qti.dcf
pm uninstall -k --user 0 com.qti.qcc
pm uninstall -k --user 0 com.qualcomm.atfwd
pm uninstall -k --user 0 com.qualcomm.atfwd2
pm uninstall -k --user 0 com.qualcomm.qti.remoteSimlockAuth
pm uninstall -k --user 0 com.spotify.music
pm uninstall -k --user 0 com.sukhavati.gotoplaying.bubble.BubbleShooter.mint
pm uninstall -k --user 0 com.tencent.soter.soterserver
pm uninstall -k --user 0 com.wdstechnology.android.kryten
pm uninstall -k --user 0 com.xiaomi.barrage
pm uninstall -k --user 0 com.xiaomi.glgm
pm uninstall -k --user 0 com.xiaomi.midrop
pm uninstall -k --user 0 com.xiaomi.mipicks
pm uninstall -k --user 0 com.xiaomi.mtb
pm uninstall -k --user 0 com.xiaomi.payment
pm uninstall -k --user 0 com.xiaomi.simactivate.service
pm uninstall -k --user 0 com.xiaomi.smarthome
pm uninstall -k --user 0 com.xiaomi.xmsfkeeper
pm uninstall -k --user 0 com.zhiliaoapp.musically
pm uninstall -k --user 0 org.ifaa.aidl.manager
```
