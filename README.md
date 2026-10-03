# Install SmartTube on ViaTV (Android TV) using ADB

A step-by-step guide to sideload [SmartTube](https://smarttube.app/) onto a ViaTV Android TV box using ADB over Wi-Fi, and make it **auto-start every time the TV boots**.

> **Disclaimer:** Do this only on a device you own. Sideloading apps may go against your ISP's terms or affect warranty/support. You do this at your own risk.

---

## Table of Contents

1. [What you need](#what-you-need)
2. [Step 1 – Enable Developer options and USB debugging](#step-1--enable-developer-options-and-usb-debugging)
3. [Step 2 – Find the TV's IP address](#step-2--find-the-tvs-ip-address)
4. [Step 3 – Download ADB (Platform Tools)](#step-3--download-adb-platform-tools)
5. [Step 4 – Connect to the TV](#step-4--connect-to-the-tv)
6. [Step 5 – Download and install SmartTube](#step-5--download-and-install-smarttube)
7. [Step 6 – Launch SmartTube](#step-6--launch-smarttube)
8. [Step 7 – Auto-start SmartTube on boot](#step-7--auto-start-smarttube-on-boot)
9. [Troubleshooting](#troubleshooting)
10. [Useful ADB commands](#useful-adb-commands)

---

## What you need

- A ViaTV Android TV box / TV
- A Windows PC (macOS/Linux also work, see notes)
- PC and TV on the **same Wi-Fi / LAN**
- The following downloads:
  - [Android SDK Platform-Tools (ADB)](https://developer.android.com/tools/releases/platform-tools)
  - [SmartTube APK](https://smarttube.app/)
  - [Launch-On-Boot APK](https://github.com/ITVlab/Launch-On-Boot) (release `v1.1.2-pre`)

---

## Step 1 – Enable Developer options and USB debugging

1. On the TV, open **Settings → Device Preferences → About**.
2. Scroll to **Build** (or *Build number*) and press **OK/Select 7 times** until you see *"You are now a developer"*.
3. Go back to **Settings → Device Preferences → Developer options**.
4. Turn **ON**:
   - **USB debugging**
   - **Network debugging / ADB over network** (if your box has this option)

> Menu names can differ slightly between ViaTV models. If you can't find *Developer options*, look under **Settings → System** or **Settings → More settings**.

---

## Step 2 – Find the TV's IP address

You can use either method:

- **On the TV:** Settings → Network → your Wi-Fi/Ethernet → note the **IP address**.
- **From your router:** Open your Wi-Fi router's admin portal (commonly `192.168.1.1` or `192.168.100.1`), go to the connected devices/DHCP list, and find the TV.

Example used in this guide: `192.168.100.224`
**Replace it with your own TV's IP.**

> Tip: Reserve a static/DHCP-reserved IP for the TV in your router so the address doesn't change.

---

## Step 3 – Download ADB (Platform Tools)

1. Go to: https://developer.android.com/tools/releases/platform-tools
2. Download **SDK Platform-Tools for Windows**.
3. **Extract** the ZIP. You will get a folder named `platform-tools`.
4. Open that folder, click the address bar, type `cmd`, and press **Enter**. A Command Prompt opens inside `platform-tools`.

*(macOS/Linux: download the matching package, extract, open a terminal in the folder, and use `./adb` instead of `adb`.)*

---

## Step 4 – Connect to the TV

In the Command Prompt:

```bash
adb connect 192.168.100.224:5555
```

A successful result looks like:

```
connected to 192.168.100.224:5555
```

**On the TV**, a popup *"Allow USB debugging?"* may appear. Tick **Always allow from this computer** and select **OK**.

Verify the connection:

```bash
adb devices
```

You should see your TV listed as `device`.

---

## Step 5 – Download and install SmartTube

1. Download the SmartTube APK from https://smarttube.app/
2. Rename it to `tube.apk` (optional but makes the command shorter) and **copy it into the `platform-tools` folder**.
3. Install it:

```bash
adb install tube.apk
```

Wait for `Success`.

Check that it's installed (lists third-party apps):

```bash
adb shell pm list packages -3
```

Look for `org.smarttube.stable` in the list.

---

## Step 6 – Launch SmartTube

ViaTV's launcher may not show sideloaded apps, so start SmartTube using ADB:

```bash
adb shell am start -n org.smarttube.stable/com.liskovsoft.smartyoutubetv2.tv.ui.main.SplashActivity
```

SmartTube should open on your TV.

---

## Step 7 – Auto-start SmartTube on boot

Since the ViaTV launcher can't be replaced, use **Launch-On-Boot** to open SmartTube automatically whenever the TV starts.

1. Go to https://github.com/ITVlab/Launch-On-Boot
2. Open **Releases** and download **`v1.1.2-pre`** → asset **`app-release.apk`** (this release fixes out-of-memory issues on devices).
3. Copy `app-release.apk` into the `platform-tools` folder.
4. Install it:

```bash
adb install app-release.apk
```

5. Launch it:

```bash
adb shell am start -n news.androidtv.launchonboot/.MainActivity
```

6. On the TV, Launch-On-Boot opens. **Select SmartTube** from the list of apps.

Done. Restart the TV, and SmartTube will open automatically on every boot.

---

## Troubleshooting

| Problem | Fix |
|---|---|
| `cannot connect to 192.168.x.x:5555` / connection refused | Make sure USB debugging / network debugging is ON, the TV and PC are on the same network, and the IP is correct. Reboot the TV and try again. |
| `device unauthorized` | Accept the debugging prompt on the TV (tick *Always allow*). If it never appears, run `adb kill-server`, then `adb connect ...` again. |
| `adb` is not recognized | You're not in the `platform-tools` folder. Open `cmd` from that folder's address bar. |
| `INSTALL_FAILED_...` error | Uninstall the older version first: `adb uninstall org.smarttube.stable`, then install again. |
| `more than one device/emulator` | Disconnect others (`adb disconnect`) or target the TV: `adb -s 192.168.100.224:5555 install tube.apk` |
| Connection drops after TV reboot | Re-run `adb connect <TV-IP>:5555`. |
| SmartTube doesn't auto-start | Reopen Launch-On-Boot (Step 7, command 5) and confirm SmartTube is selected. |

---

## Useful ADB commands

```bash
adb devices                                  # list connected devices
adb disconnect                               # disconnect all
adb kill-server                              # restart the ADB server
adb shell pm list packages -3                # list user-installed apps
adb uninstall org.smarttube.stable           # uninstall SmartTube
adb uninstall news.androidtv.launchonboot    # uninstall Launch-On-Boot
```

---

## Credits

- [SmartTube](https://smarttube.app/) – ad-free YouTube client for Android TV
- [Launch-On-Boot](https://github.com/ITVlab/Launch-On-Boot) by ITVlab / Fleker
- [Android SDK Platform-Tools](https://developer.android.com/tools/releases/platform-tools) by Google

---

*If this guide helped you, consider giving the repo a ⭐.*
