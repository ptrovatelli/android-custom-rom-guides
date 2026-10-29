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

You should know that trying to install a custom ROM could brick your phone and make it useless, although there are often ways to fix an apparently bricked phone, providing you have the time and skills.

# Official instructions

As a reference, find below the official sources of information: 

- [Page for this device on evolution-x official website](https://evolution-x.org/device/op6893)
- [xda forum page for evolution-x on this device](https://xdaforums.com/t/rom-op6893-a16-evolutionx-official-for-oneplus-nord-2-5g-and-realme-x7-max-5g.4796652/)

- [telegram support channel](http://t.me/OnePlusNord2GlobalOfficial) 

# Pre-requisites

## Handle your data and primary phone

If this is your primary phone, you may want to get another phone while you're working on this one because it may take time for everything to work as you would expect. 

I have not documented how to backup/restore the data because I haven't tested this but it's possible to make a full backup of your phone

What I have done: 

- Backed up my photos/videos to external drive (always a good idea)
- Got another phone. Transfered all data to this other phone after a factory reset.

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

**Beware that there are 2 regional variants of this device**: 

- DN2103 for europe
- DN2101 for India
  - I haven't tested this variant so I can't provide a working URL. Search on the way back machine, on forums, ask on telegram...

Make sure your use the correct package. You can double-check your phone variant in the phone parameters.

## Download the downgrade package 

One plus has stopped distributing this package, so we'll find it from the internet archive (aka wayback machine)

I have tried several packages before getting one that works (some displayed *incompatible with your system* upon installation)

This one worked for the EU device: https://web.archive.org/web/20230207132242/https://oxygenos.oneplus.net/oplus_ota_downgrade_EU.zip

This is the latest version stored on the wayback machine

``` 
sha256sum 'oplus_ota_downgrade_EU'
3db05626498716191c570dadc08d90adcca0fca5c35a997778a34454c6f64ead  oplus_ota_downgrade_EU.zip
du -h 'oplus_ota_downgrade_EU.zip'
3,6G	oplus_ota_downgrade_EU.zip
```

Some say that the package needs to be renamed to `oplus_ota_downgrade.zip` but I don't think it did a difference. 

## Transfer package to the phone

Copy the file to the root of your phone storage (plug the phone via usb to be PC for example and when the phone asks what to do with this connexion choose "transfer files")

## Install downgrade package

We'll install the package using **local install**

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

