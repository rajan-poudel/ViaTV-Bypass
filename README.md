# Install SmartTube on a Restricted ViaTV Android TV Box









+











+

This guide explains how to install and launch **SmartTube** on a ViaTV Android TV box when the provider UI does not expose YouTube, Google Play Store, or a browser.









+











+

> **Use this only on a device you own or are explicitly authorized to administer.**









+

> This guide does not bypass account passwords, DRM, paid services, device encryption,









+

> or provider authentication. It uses Android Debug Bridge (ADB) only when USB/network









+

> debugging has already been enabled on the device.









+











+

## What this does









+











+

The procedure:









+











+

1. Enables Android developer options and ADB debugging.









+

2. Connects a Windows computer to the TV box over the local network.









+

3. Installs SmartTube from its official website.









+

4. Starts SmartTube directly, even when the ViaTV launcher does not show it.









+

5. Optionally installs a launch-on-boot utility so SmartTube starts when Android TV boots.









+











+

It does **not** remove or modify the ViaTV system application.









+











+

## Requirements









+











+

- The ViaTV box and Windows PC connected to the same trusted Wi-Fi/LAN.









+

- Permission to administer the box.









+

- A USB keyboard or the box remote, if needed to navigate settings.









+

- Windows administrator permission only if Windows asks for it.









+

- At least a few hundred MB of free storage.









+











+

The box must support Android Debug Bridge over the network. Some provider-managed









+

firmware disables this feature; if so, do not attempt to defeat that restriction.









+











+

## 1. Enable developer options and USB debugging









+











+

On the TV box:









+











+

1. Open **Settings**.









+

2. Open **Device Preferences**, **System**, or **About**. The wording varies by firmware.









+

3. Find **Build**, **Build number**, or a similar software-version entry.









+

4. Press **OK** repeatedly (usually seven times) until Android says developer options are enabled.









+

5. Return to Settings and open **Developer options**.









+

6. Enable **USB debugging**. On some firmware the relevant option is named









+

   **ADB debugging**, **Network debugging**, or **USB debugging (Security settings)**.









+

7. If the box shows an authorization prompt, approve the computer only if it is yours.









+











+

Do not enable debugging on an untrusted network. ADB can provide extensive control over









+

the device while it is enabled.









+











+

## 2. Find the box IP address









+











+

Open the box's Wi-Fi or network settings and note its IPv4 address. It commonly looks









+

like `192.168.100.224`.









+











+

You can also check the router's connected-device/client list. Use the box's current









+

address; it may change after a reboot unless the router reserves it.









+











+

## 3. Download Android Platform-Tools for Windows









+











+

Download **SDK Platform-Tools for Windows** from Google's official page:









+











+

<https://developer.android.com/tools/releases/platform-tools>









+











+

Extract the ZIP file to a convenient location, for example:









+











+

```text









+

C:\Users\<your-user>\Desktop\platform-tools









+

```









+











+

Do not download `adb.exe` from an unofficial mirror.









+











+

## 4. Download SmartTube









+











+

Download SmartTube only from its official website:









+











+

<https://smarttube.app/>









+











+

Use the Android TV-compatible APK offered there. Save or copy the APK into the









+

`platform-tools` folder and rename it to `tube.apk` if desired.









+











+

Before installing, verify that the downloaded file is an APK and that it came from









+

the official SmartTube source. Do not install a random APK advertised as SmartTube.









+











+

## 5. Connect ADB to the box









+











+

Open **Command Prompt** (not a browser address bar) and change to the extracted folder:









+











+

```bat









+

cd /d "%USERPROFILE%\Desktop\platform-tools"









+

```









+











+

Replace the path if you extracted Platform-Tools elsewhere. Connect using the IP address









+

you found earlier:









+











+

```bat









+

adb connect 192.168.100.224:5555









+

adb devices









+

```









+











+

Expected output includes:









+











+

```text









+

192.168.100.224:5555    device









+

```









+











+

If the box displays an RSA authorization prompt, select **Allow**. If the status is









+

`unauthorized`, approve the prompt on the TV and run `adb devices` again.









+











+

If the connection fails:









+











+

- Confirm the PC and box are on the same Wi-Fi/LAN.









+

- Confirm the IP address has not changed.









+

- Confirm debugging is enabled.









+

- Disconnect from a guest Wi-Fi network that blocks device-to-device traffic.









+

- Try `adb disconnect` and then reconnect.









+











+

## 6. Install SmartTube









+











+

With the APK named `tube.apk` in the current Platform-Tools folder, run:









+











+

```bat









+

adb install "tube.apk"









+

```









+











+

Successful installation ends with:









+











+

```text









+

Success









+

```









+











+

If Android reports that the app is incompatible, use the correct Android TV APK from









+

the official SmartTube download page. Do not randomly downgrade system components or









+

factory-reset the box.









+











+

Confirm that the package is installed:









+











+

```bat









+

adb shell pm list packages -3









+

```









+











+

You should see:









+











+

```text









+

package:org.smarttube.stable









+

```









+











+

## 7. Launch SmartTube









+











+

Use this exact command:









+











+

```bat









+

adb shell am start -n org.smarttube.stable/com.liskovsoft.smartyoutubetv2.tv.ui.main.SplashActivity









+

```









+











+

You can also launch it with Android's package launcher:









+











+

```bat









+

adb shell monkey -p org.smarttube.stable 1









+

```









+











+

The first launch may take a little longer on a low-storage or low-memory device.









+











+

### Important command-line details









+











+

- Type `adb`, not `db`.









+

- Run each command at a normal prompt such as `C:\...\platform-tools>`.









+

- Do not type the prompt text itself.









+

- In Command Prompt, a caret (`^`) continues a command onto the next line. For fewer









+

  mistakes, use one complete line per command.









+

- If `adb` says no devices are found, reconnect:









+











+

  ```bat









+

  adb connect 192.168.100.224:5555









+

  adb devices









+

  ```









+











+

## 8. Optional: start SmartTube automatically at boot









+











+

SmartTube itself may not start automatically on a restricted provider launcher. An









+

optional third-party utility is **Launch-On-Boot**:









+











+

<https://github.com/ITVlab/Launch-On-Boot>









+











+

Use the project's release page and download the APK for release **v1.1.2-pre**,









+

if it is compatible with the Android version on the box. The release is old, so









+

compatibility is not guaranteed.









+











+

Copy the downloaded release APK, commonly named `app-release.apk`, into the









+

Platform-Tools folder. Install it:









+











+

```bat









+

adb install "app-release.apk"









+

```









+











+

Try to open its main screen:









+











+

```bat









+

adb shell am start -n news.androidtv.launchonboot/.MainActivity









+

```









+











+

If that activity name is not available, ask Android for the actual launchable activity:









+











+

```bat









+

adb shell cmd package resolve-activity --brief -a android.intent.action.MAIN -c android.intent.category.LEANBACK_LAUNCHER -p news.androidtv.launchonboot









+

adb shell cmd package resolve-activity --brief -a android.intent.action.MAIN -c android.intent.category.LAUNCHER -p news.androidtv.launchonboot









+

```









+











+

If a component is returned, launch it with:









+











+

```bat









+

adb shell am start -n PACKAGE/ACTIVITY









+

```









+











+

You can also try:









+











+

```bat









+

adb shell monkey -p news.androidtv.launchonboot 1









+

```









+











+

In Launch-On-Boot, select **SmartTube** and enable the option to launch the selected









+

app at boot. Reboot the box and verify the behavior.









+











+

If Android reports **No activity found**, the installed APK does not expose a









+

launchable activity on this firmware. Do not keep guessing activity names; inspect the









+

package or use a newer compatible release instead.









+











+

## 9. Verify and clean up









+











+

Check installed user packages:









+











+

```bat









+

adb shell pm list packages -3









+

```









+











+

When finished, disable debugging on the box if you do not need it:









+











+

1. Open **Settings → Developer options**.









+

2. Turn off **USB debugging** or **ADB/network debugging**.









+











+

Then disconnect the PC:









+











+

```bat









+

adb disconnect 192.168.100.224:5555









+

```









+











+

## Troubleshooting









+











+

### `Error type 3` or `Activity class ... does not exist`









+











+

The package may be installed under a different version, or the activity name is wrong.









+

Find the installed package and resolve its launcher activity:









+











+

```bat









+

adb shell pm list packages -3









+

adb shell cmd package resolve-activity --brief -a android.intent.action.MAIN -c android.intent.category.LEANBACK_LAUNCHER -p org.smarttube.stable









+

```









+











+

### `Unable to find package`









+











+

The package is not installed on the currently connected device. Confirm the target:









+











+

```bat









+

adb devices









+

adb install "tube.apk"









+

```









+











+

### SmartTube is installed but not visible in the ViaTV app list









+











+

That is expected on a restricted launcher. Start it with the ADB command in section 7,









+

or use a compatible launcher utility. The app can be installed even when the provider









+

launcher hides it.









+











+

### Installation is blocked









+











+

The provider firmware may disallow unknown-source installation or may enforce device









+

management policies. Contact ViaTV or use an external streaming device rather than









+

trying to defeat the policy.









+











+

### The box becomes unstable or storage is full









+











+

Uninstall only the optional apps you installed:









+











+

```bat









+

adb uninstall news.androidtv.launchonboot









+

adb uninstall org.smarttube.stable









+

```









+











+

Do not uninstall ViaTV system packages. A factory reset can remove activation data and









+

may make the box require provider support, so use it only as a last resort and only









+

with the provider's instructions.









+











+

## Alternative if this is blocked









+











+

If ADB installation or app launching is disabled by the provider, the reliable options









+

are to ask ViaTV to enable YouTube/provide a replacement box, use a separate Google TV,









+

Android TV, Roku, or Fire TV device, or connect a computer to the television by HDMI.
