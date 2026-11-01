- [Introduction](#introduction)
  - [What is this](#what-is-this)
  - [Disclaimer](#disclaimer)
- [Official instructions](#official-instructions)
- [Pre-requisites](#pre-requisites)
  - [Handle your data and primary phone](#handle-your-data-and-primary-phone)
  - [Install tools on ubuntu](#install-tools-on-ubuntu)
  - [Activate developer mode on your phone and more](#activate-developer-mode-on-your-phone-and-more)
- [Downgrade to OxygenOS 11](#downgrade-to-oxygenos-11)
  - [Device variants](#device-variants)
  - [Download the downgrade package](#download-the-downgrade-package)
  - [Transfer package to the phone](#transfer-package-to-the-phone)
  - [Install downgrade package](#install-downgrade-package)
- [Install firmware and custom ROM](#install-firmware-and-custom-rom)
  - [Unlock bootloader](#unlock-bootloader)
  - [Download TWRP, firmware, ROM](#download-twrp-firmware-rom)
  - [Flash TWRP](#flash-twrp)
  - [Flash firmware and ROM](#flash-firmware-and-rom)
  - [Format data](#format-data)
  - [Reboot](#reboot)
- [Play integrity](#play-integrity)
  - [Verify current play integrity status](#verify-current-play-integrity-status)
  - [Play integrity (device, strong)](#play-integrity-device-strong)
- [Testing sheet](#testing-sheet)
  - [Conditions](#conditions)
  - [Communication](#communication)
  - [Peripherals](#peripherals)
  - [Phone basics](#phone-basics)
  - [Applications](#applications)


# Introduction

## What is this

This is a step by step guide on how to install **evolution-x rom on a one plus nord 2** phone currently on stock rom, with the lastest patched applied.

The latest patches date early 2026 as this device is not receiving any updates on the stock android (**stock device stuck at android 13 security patch january 2026** DN2103_11_F.59)

This article assumes that you have an **ubuntu PC** at hand. Of course windows/mac/other linux is possible but some commands will need to be adapted

In evolution-x, this device is in the `op6893` tree, device `denniz`. 

At the time of writing this article, evolution-x is based on android 16, with monthly security patches applied

Please feel free to open issues or merge requests to fix anything.

## Disclaimer

I'm not an expert and there may be other ways to do it.

This gives more detailed instructions than the official documentation and could be of great help if you've never installed a custom ROM before.

You should know that trying to install a custom ROM could brick your phone and make it useless, although there are often ways to fix an apparently bricked phone, provided you have the time and skills.

# Official instructions

As a reference, find below the official sources of information: 

- [Page for this device on evolution-x official website](https://evolution-x.org/device/op6893)
- [xda forum page for evolution-x on this device](https://xdaforums.com/t/rom-op6893-a16-evolutionx-official-for-oneplus-nord-2-5g-and-realme-x7-max-5g.4796652/)

- telegram channels:
  - [telegram support channel for One plus nord 2](http://t.me/OnePlusNord2GlobalOfficial)  (linked by xda page)
    - In the channel, type `/notes` to get a list of tutorials
  - [telegram evolution-x channel](https://t.me/EvolutionX)

- [evolution-x github repository](https://github.com/evolution-x)

- [evolution-x wiki](https://wiki.evolution-x.org/)

  

This article is worth mentioning too, not sure everything is up to date though: [Nord2Guides.md](https://gist.github.com/Giovix92/c7e69123dbc51adf3f3a26f9c3c0208e#file-nord2guides-md)

# Pre-requisites

## Handle your data and primary phone

If this is your primary phone, you may want to get another phone while you're working on this one because it may take time for everything to work as you would expect. 

I have not documented how to backup/restore the data because I haven't tested this but it's possible to make a full backup of your phone

What I have done: 

- Backed up my photos/videos to external drive (always a good idea)
- Got another phone. Transfered all data to this other phone after a factory reset using android native feature (after the factory reset, the new phone will ask if you want to get data from another phone)

## Install tools on ubuntu

Install adb and fastboot to communicate with the phone:

```
sudo apt-get install android-tools-adb
sudo apt-get install android-tools-fastboot
```

## Activate developer mode on your phone and more

Step 1: Enable Developer Mode

1. Open the **Settings** app on your phone.
2. Scroll down and tap on **About device**.
3. Select **Version**.
4. Find **Version number** (or Build number) and tap it **7 times** in a row.
5. Enter your device's **PIN, pattern, or password** when prompted.
6. You will see a confirmation message at the bottom stating: *"You are now in developer mode"* (or *"You are now a developer"*).

Step 2: Enable USB Debugging

1. Go back to the main **Settings** menu.
2. Scroll down and tap on **System & updates** (or **Additional settings** depending on your sub-version).
3. Select **Developer options**.
4. Scroll down the list to find **USB debugging** and tap the toggle switch to turn it on.
5. Tap **OK** on the security warning prompt to confirm.

Step 3: OEM unlocking (although this doesn't seem to have really any effect in my experience, unlocking needs to be done with fastboot too, see later)

1. Open **Settings**.
2. Tap on **Additional settings** (or **System & updates**).
3. Open **Developer options**.
4. Scroll down a bit and locate **OEM unlocking** (or *Enable OEM unlock*).
5. Toggle the switch to **On**.
6. Enter your lock screen **PIN or password** to authorize the change.
7. Tap **Enable** on the confirmation warning pop-up.

# Downgrade to OxygenOS 11

OxygenOS 13 has locked the possibility to enter fastboot / bootloader mode. In order to do this, you first need to revert to OxygenOS 11.

## Device variants

**Beware that there are 2 regional variants of this device and 3 downgrade packages**: 

- DN2103 for europe (**EU**) and other countries - not india, not europe- (**GLO**)
- DN2101 for **India**

Make sure your use the correct package. You can double-check your phone variant in the phone parameters.

## Download the downgrade package 

One plus has stopped distributing this package, so we'll find it from the internet archive (aka wayback machine)

Original (dead) links were found from https://community.oneplus.com/thread/1596759 and fed to the Wayback machine.

Direct (working) links: 

- DN2103 / EU : https://web.archive.org/web/20230207132242/https://oxygenos.oneplus.net/oplus_ota_downgrade_EU.zip

```
sha256sum 'oplus_ota_downgrade_EU'
3db05626498716191c570dadc08d90adcca0fca5c35a997778a34454c6f64ead  oplus_ota_downgrade_EU.zip
du -h 'oplus_ota_downgrade_EU.zip'
3,6G	oplus_ota_downgrade_EU.zip
```

- DN2103 / GLO: https://web.archive.org/web/20221117114244/https://oxygenos.oneplus.net/oplus_ota_downgrade_NA.zip (to be tested)
- DN2101 / India: https://web.archive.org/web/20230207132242/https://oxygenos.oneplus.net/oplus_ota_downgrade_Nord2Rollback.zip (to be tested)

Rename to `oplus_ota_downgrade.zip` after download



Alternatively, you can check this method in telegram notes: [download_oplus_rollback](http://t.me/MissRose_bot?start=notes_-1001596273500_1944257)

## Transfer package to the phone

Copy the file to the root of your phone storage (plug the phone via usb to the PC for example and when the phone asks what to do with this connexion choose "transfer files")

## Install downgrade package

We'll install the package using **local install**

See also in telegram */notes* : 

- [local_install_fix](http://t.me/MissRose_bot?start=notes_-1001596273500_1944259)
- [downgrade_instructions](http://t.me/MissRose_bot?start=notes_-1001596273500_1409105)



1. **The "Anti-Crash" protocol in settings:**
   - Enable **Airplane Mode** on your phone (crucial to cut off Wi-Fi and mobile data).
   - Go to *Settings > Apps > App management*.
   - Tap the three dots in the top-right corner and select **"Show system"**.
   - Look for the app named **"Software Update"** (or *com.oplus.ota*).
   - Go to *Storage usage* and tap **Clear data**.
2. **Initiate the Downgrade:**
   - **Keep Airplane Mode enabled**.
   - Go to *Settings > About device*.
   - Tap the large **OxygenOS** banner at the top (the menu that usually checks for updates).
   - Tap the **three dots** in the top-right corner.
   - The **Local install** option will no longer be greyed out and will become visible.
   - Select your `.zip` file and begin the installation.

3. Launching the Downgrade

   - Open the **System Update** app (which was just installed, either named *System Update* or showing a gear icon).

   - Tap the **gear icon** (or the three dots) in the top-right corner.

   - Select **Local upgrade**.

   - The app will automatically detect the downgrade `.zip` file you placed in the root directory during Step 1. Select it.

   - A warning message will appear to remind you that all your data will be erased. Confirm the action.

   - Let the installation process run until it finishes (this can take 5 to 10 minutes).

   - As soon as the installation reaches 100%, tap **Restart**.

# Install firmware and custom ROM

## Unlock bootloader

After reboot to OOS 11, configure briefly your phone without setting your accounts:

1. Do again [Activate developer mode on your phone and more](#activate-developer-mode-on-your-phone-and-more)

2. Plug the phone to your PC

3. Run

   ```
   adb reboot bootloader
   ```

Phone should enter **Fastboot Mode**. 

On ubuntu type in: 

``````
fastboot flashing unlock
``````

Validate with volume up.

## Download TWRP, firmware, ROM

- Download the latest TWRP image from https://dl.twrp.me/denniz/ 
- Download firmware from https://archive.org/download/rmx3031-firmware-f-57-restore-brom/13.0-F.57-Denniz_Restore_Brom_Firmware.zip
- Download evolution-x ROM from https://evolution-x.org/device/op6893

## Flash TWRP

TWRP is an app that will help us install the firmware and the ROM to the device

Switch to fastbootd mode: 

```
fastboot reboot fastboot
```

Flash the TWRP image to the recovery partition with:  

```
fastboot flash recovery twrp-3.7.0_11-0-denniz.img
```

Reboot the phone to recovery to run TWRP on the phone:

```
fastboot reboot recovery
```

## Flash firmware and ROM

As we are now on OxygenOs 11 and we want to install an android 16 ROM, we need to first update the firmware

**You must flash both the firmware and the ROM without rebooting in between**

In TWRP, before starting wipe data

- wipe / advanced : 

  - dalvik

  - data

  - cache

- Go to Advanced / ADB Sideload
- Swipe the "**Swipe to Start Sideload**" cursor so that twrp will wait for the PC to push an image

On ubuntu push the firmware using adb:

```
adb sideload 13.0-F.57-Denniz_Restore_Brom_Firmware.zip
```

Wait for TWRP to finish processing it, then push the ROM: 

```
adb sideload EvolutionX-16.0-20260919-op6893-11.11-Official.zip
```

## Format data

In TWRP, go to **wipe menu / format data**

## Reboot

Reboot. 

The Phone should now run Evolution-x!

# Play integrity
## Verify current play integrity status

Download and install **Play Integrity API Checker** from the Play Store.

Initial status: 

- MEETS_BASIC_INTEGRITY: green
- MEETS_DEVICE_INTEGRITY: red
- MEETS_STRONG_INTEGRITY: red

## Play integrity (device, strong)

There seems to be an issue with the spoofing components at the moment (end October / November 2026) after some changes on google side.

Reaching device or strong integrity is probably doable with rooting + specific modules (just what you would do on any other ROM)

# Testing sheet

## Conditions

Tested on evolution-x 11.11, build BP4A.251205.006.

Device One plus nord 2 DN2103

## Communication

| What                     | Test                                                         | Result |
| ------------------------ | ------------------------------------------------------------ | ------ |
| wifi                     | Cut out data and access internet on wifi                     | OK     |
| internet access (4g)     | (switch favorite network to 4G if you're stuck on 5G). (see note 1) | OK     |
| internet access (5g)     | (see note 1)                                                 | OK     |
| hotspot (4G->wifi)       | (switch favorite network to 4G if you're stuck on 5G).       | OK     |
| hotspot (5g->wifi)       |                                                              | OK     |
| hotspot (wifi->wifi)     |                                                              | OK     |
| VOLTE (voice over 4G)    | Make sure you're now on 4G (switch favorite network to 4G if you're stuck on 5G).<br/>Initiate a call and keep it active for 10 seconds.<br/>Make sure you're still on 4G | OK     |
| VONR (voice over 5G)     | Make sure you're now on 5G.<br/>Initiate a call and keep it active for 10 seconds.<br/>Make sure you're still on 5G | OK     |
| VOWIFI (voice over wifi) | Check option is present in parameters (was deactivated out of the box)<br/>Cut out data. Make a call. Check that "WIFI call in progress" is displayed (hard to be 100% sure) | OK     |

**Note 1**: if you have an exclamation mark besides your "4G/5G" icon, check your APN settings in order to select your correct carrier. It may be incorrect out of the box

## Peripherals

| What               | Test                                                         | Result |
| ------------------ | ------------------------------------------------------------ | ------ |
| gps                | GPS guiding in a car                                         | TODO   |
| camera             | Quick test: take one picture front camera, one back camera. Take a quick movie | OK     |
| nfc                | TODO                                                         | TODO   |
| fingerprint sensor | Configure phone unlock with fingerprints                     | OK     |
| facial recognition | Configure phone unlock with facial recognition               | OK     |

## Phone basics

| What         | Test                                                         | Result |
| ------------ | ------------------------------------------------------------ | ------ |
| vibrate mode | Receive call with vibrate mode                               | OK     |
| warp charge  | Make sure phone completely charge within about 20mn with appropriate cable | OK     |

## Applications

| What           | Test                                        | Result |
| -------------- | ------------------------------------------- | ------ |
| Play integrity | Download and run Play Integrity API Checker | Basic  |
| google apps    | TODO                                        | TODO   |

