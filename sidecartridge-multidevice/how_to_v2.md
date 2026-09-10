---
layout: default
title: How to
nav_order: 9
nav_exclude: false
parent: SidecarTridge Multi-device
redirect_from:
  - /how_to_v2
  - /how_to_v2/
---

# How to
{: .no_toc }

This section showcases a variety of different configurations and parametric setups for the SidecarTridge Multi-device board. You don't need to be a developer to follow these guides, but you will need to be familiar with the basics of the Atari ST and the Multi-device board. If you are new to the Multi-device project, please refer to the [Getting Started](/sidecartridge-multidevice/getting_started_v2/) section first.

<details open markdown="block">
  <summary>
    Table of contents
  </summary>
  {: .text-delta }
1. TOC
{:toc}
</details>

## Update the firmware

There are three ways to update the Multi-device firmware. Try them in this order: the over-the-air update from the Booster app first, the browser-based web installer if the device cannot update itself, and the manual BOOTSEL procedure as the last resort.

### Option 1: Over-the-air update from the Booster app (recommended)

Starting in firmware version **v2.0.6 Beta**, you don't need to download anything manually. When a new firmware version is available, the Booster app shows a banner on top of its web interface, including the target version and a link to the changelog.

To update, click the **Continue upgrading** button in the web interface. The Booster app downloads the new firmware version to the microSD card, which takes about two minutes, and the screen of the Atari 16/32 computer displays the progress. Once the download finishes, click the **Confirm firmware upgrade** button to reflash the Raspberry Pi Pico W. After a few seconds the device reboots with the new firmware.

The full process with screenshots is described in the [Firmware update section of the User Guide](/sidecartridge-multidevice/userguide_v2/#firmware-update).

{: .warning }
If the update process fails or gets interrupted, the device can get bricked. In that case, reinstall the firmware with one of the two methods below.

### Option 2: Using the browser-based web installer

If the over-the-air update is not available or the device needs a fresh installation, use the [SidecarTridge Firmware Installer](https://webflash.sidecartridge.com/) in a Chromium-based browser such as Google Chrome or Microsoft Edge (Firefox and Safari are not supported):

1. Open the [SidecarTridge Firmware Installer](https://webflash.sidecartridge.com/) in Chrome or Edge.
2. Unplug the Raspberry Pi Pico W, hold the **BOOTSEL** button, plug the micro USB cable back into your computer, and then release the **BOOTSEL** button. Your computer should recognize the device as the mass storage device `RPI-RP2` (RP2040) or `RP2350`.
3. In the installer, select the device family and the firmware version, then click **Detect MCU** and grant USB access when the browser asks for it.
4. Click **Flash Start**. You can leave the **Erase before write** and **Verify after write** options enabled for a clean install.
5. Wait until the flashing finishes, then unplug the board.

See the [Firmware Installation section of the Getting Started guide](/sidecartridge-multidevice/getting_started_v2/#option-1-using-the-browser-based-installer-recommended) for more details.

### Option 3: Manual update when the web installer does not work

If the web installer does not detect your board or your browser does not support WebUSB, you can always flash the firmware manually:

1. Download the latest STABLE firmware file (`.uf2`) from the [Multi-device download page](https://sidecartridge.com/downloads/sidecartridge-multidevice-atari-st/).
2. Connect your PC, Mac, or Linux to the Raspberry Pi Pico W using the USB cable. Now hold down the BOOTSEL button for a few seconds and then hold down the RESET button. Now release the RESET button and gently release the BOOTSEL button afterwards. A new drive named `RPI-RP2` should appear on your PC, Mac, or Linux. Open it.

{:refdef: style="text-align: center;"}
![How enter in BOOTSEL mode](https://sidecartridge.com/assets/images/quickstart/bootsel-mode.gif)
{: refdef}

{:start="3"}
3. Copy the downloaded `.uf2` file corresponding to your Raspberry Pi Pico W into the `RPI-RP2` drive. Wait for the file to be copied.
4. Disconnect the USB cable. The new firmware has now been flashed.

## Fully erase the flash of the device

Sometimes the device can end up with corrupted information in the areas of the flash memory used for the global configuration of the device. If you run into persistent problems that a firmware reinstall does not fix, a good option is to completely erase the content of the flash memory and start from scratch.

The easiest way to do this is with the well-known `flash_nuke` firmware from Raspberry Pi, a tiny program that erases the entire flash memory of the board. It is part of the official [pico-examples repository](https://github.com/raspberrypi/pico-examples/tree/master/flash/nuke) and Raspberry Pi publishes a ready-to-use binary.

1. Download the [flash_nuke.uf2](https://datasheets.raspberrypi.com/soft/flash_nuke.uf2) file from Raspberry Pi. This universal build works on both RP2040 and RP2350 boards.
2. Put the board in BOOTSEL mode as described in the manual update procedure above: connect the USB cable while holding the **BOOTSEL** button, until the `RPI-RP2` drive appears on your computer.
3. Copy the `flash_nuke.uf2` file into the `RPI-RP2` drive. Wait for the file to be copied. The board will erase the entire flash memory and reboot back into BOOTSEL mode, so the `RPI-RP2` drive will appear again.
4. Install the firmware from scratch using the [web installer](#option-2-using-the-browser-based-web-installer) or the [manual procedure](#option-3-manual-update-when-the-web-installer-does-not-work) described above.

{: .warning }
This operation deletes everything stored in the flash memory of the device: the firmware, the global configuration, and any microfirmware apps installed in flash. After the erase you must reinstall the firmware and configure the device again from scratch, including the WiFi settings.

## Reset the WiFi configuration and return to Factory mode

If you need to reconfigure the WiFi of the device from scratch, for example after changing your router or moving the device to another network, you can reset it to **Factory mode**. There are two ways to do it.

### Option 1: From the Booster web interface (easiest)

If the device is still reachable on the network, open the Booster web interface and go to the [Device view](/sidecartridge-multidevice/userguide_v2/#device-view). At the bottom of the view there is a **Restore to the default fabric settings** button: click it and the device reboots and loads the Booster app in **Factory mode**. Then continue with step 3 below to reconfigure the WiFi.

### Option 2: With the SELECT button

Use this option when the device is not reachable over the network (for example, the WiFi credentials are no longer valid):

1. With the device powered on, press and hold the **SELECT** button for more than 10 seconds.
2. Power off and power on the device and the computer.
3. The device starts in **Factory (Fabric) mode** and broadcasts its own WiFi network named `SIDECART` with password `sidecart`.
4. Connect to the `SIDECART` network from a computer or smartphone and open `http://192.168.4.1` in a web browser.
5. Select your WiFi network from the list, enter the password and click **Connect**. The device saves the credentials to flash memory, reboots and connects to your network.

Once connected, the web interface is reachable at `http://sidecart.local` if your network supports mDNS, or at the IPv4 address shown on the computer screen. See the [Network view section of the User Guide](/sidecartridge-multidevice/userguide_v2/#network-view) for all the network configuration options.

## What to do when the Booster cannot connect to WiFi

If the Booster app fails to connect to your WiFi network, check the following points:

- The device only supports **2.4 GHz WiFi networks**. 5 GHz networks are not supported by the onboard Raspberry Pi Pico W hardware, so make sure your router broadcasts a 2.4 GHz network and that you are selecting that one.
- Check the signal strength. Enable the **Show RSSI** option in the [Network view](/sidecartridge-multidevice/userguide_v2/#network-view) to display the RSSI value in dBm of the visible WiFi networks. Below `-80 dBm` the signal is almost unusable: the board may fail to connect, lose the connection intermittently or take longer to obtain an IP address. Move the device closer to the access point or choose a network with a stronger signal.
- While connected, the device replies to ICMP ping requests, so you can use `ping sidecart.local` (or the IP address shown on screen) from another computer to verify the connection.

If the device connects to the WiFi network but does not obtain an IP address, there are two usual causes:

1. **The device cannot obtain an IP address from the DHCP server of the network.** This can happen because there is no DHCP server on the network, or because the router applies some kind of filter, such as MAC address filtering, that prevents the device from getting a lease. Check the DHCP and filtering configuration of your router, or configure a static IPv4 address from the [Network view](/sidecartridge-multidevice/userguide_v2/#network-view).
2. **The authentication method is not correct.** The device tries to detect the authentication method of the network automatically, but in some networks you may need to select it manually. Note that the supported method is not necessarily the one announced by the router, so if the connection fails try the other authentication methods available.

Starting with Booster v2.1.0, if the WiFi negotiation fails the Manager falls back to an offline-safe mode: the terminal on the Atari computer remains active so you can still boot the microfirmware apps that are already downloaded. Press `ESC` on the terminal to enter the apps workflow, or hold any `SHIFT` key to keep booting from GEMDOS without touching the web interface.

If you need to reconfigure the WiFi from scratch, follow the [Reset the WiFi configuration](#reset-the-wifi-configuration-and-return-to-factory-mode) procedure above.

## Improve the WiFi signal and stability

If the device connects but downloads fail intermittently or the connection drops, a weak or unstable WiFi signal is the most common cause. All the relevant settings live in the [Network view](/sidecartridge-multidevice/userguide_v2/#network-view) of the Booster web interface:

- **Country**: the default is the worldwide profile. Select your actual country so the WiFi module uses the channels and frequencies allowed in your region, which usually improves signal quality.
- **Wifi Power**: adjusts the power profile of the WiFi module, from 0 to 4. Higher values keep the radio fully powered, which improves stability during long downloads at the cost of some extra power consumption.
- **Show RSSI**: enable it to display the signal strength in dBm of the visible networks. Use the value reported by the device itself, not the one shown by your phone or laptop: each radio measures its own reception, and the device is the one doing the downloads.

Remember to click **Save** after changing any of these settings, and power the device off and on so they take effect.

General measures that also help: move the router or the computer closer, avoid metal enclosures or shelves between them, and reduce interference from other 2.4 GHz sources.

### Download apps with the device outside the computer

When the WiFi signal at the computer's location is too weak to complete downloads, you can provision the device right next to the router:

1. Power off the computer and remove the device from the cartridge port.
2. Power the device on its own with a micro-USB cable, close to the router.
3. Open the Booster web interface from a browser at `http://sidecart.local` (or the device IP address).
4. Download every microfirmware app you need from the [Apps view](/sidecartridge-multidevice/userguide_v2/#apps-view).
5. Power off the device, install it back in the cartridge port and boot the computer.
6. Launch the downloaded apps from the Booster menu. Launching an already-downloaded app does not need WiFi.

## Verify the device hardware with the test ROM

If you suspect a hardware problem with the device or with the connection to the cartridge port of your computer, you can run the **Multidevice Test** suite before drawing any conclusion. The test performs several read tests against the emulated ROM banks and shows the results on screen, which helps discriminate between a faulty device, a dirty or damaged cartridge port, and a software issue.

1. Download the `TESTSCRT.TOS` and `TESTROM.BIN` files from the [md-testrom releases page](https://github.com/sidecartridge/md-testrom/releases) and copy both files to the same directory on your Atari computer.
2. In the Booster web interface, find the **Multidevice Test** microfirmware in the Apps view, install it and launch it.
3. Boot the Atari computer and run the `TESTSCRT.TOS` program. The full test run takes several minutes.
4. A healthy device passes all the tests without errors. Any failing test points to a potential issue with the device, the connection to the cartridge port, or the computer itself.
5. To return to the Booster app, press the **SELECT** button on the device.

The [md-testrom repository](https://github.com/sidecartridge/md-testrom) documents the full procedure, including how to set up the device in blind mode when the computer does not boot with the device connected.

## Format the microSD card

To use the Multi-device effectively, your microSD card needs to be formatted in FAT32 or exFAT, using the standard layout with a single partition. Cards with multiple partitions are not supported, and neither are old SD standard cards of 2GB or less: only SDHC, SDXC and SDUC cards work. **We strongly recommend using a high-quality SDHC, SDXC or SDUC microSD from a reputable brand** to ensure optimal performance and reliability. To format the microSD card, you can use the [SD Card Formatter](https://www.sdcard.org/downloads/formatter/) tool available for PC/Mac/Linux, which restores the standard single-partition layout.

{: .note }
Always ensure you've selected the correct device to format, especially when working with disk utilities, to avoid data loss.
{: .note }

## Install or downgrade a specific version of a microfirmware app

When the catalog exposes more than one version of a microfirmware app, the app card in the [Apps view](/sidecartridge-multidevice/userguide_v2/#apps-view) of the Booster web interface shows a **Version** dropdown. The newest version is selected by default, but you can pick any older version still published in the catalog. When the selected version is not the one currently installed, the card also indicates which version is already present on the device.

The action button next to the app is context aware: it reads **Install**, **Update** or **Downgrade** depending on whether the selected version is new, newer than the installed one, or older than the installed one. Downgrading to an older version requires an explicit confirmation step.

This is useful to roll back to a previous version of an app when you hit a regression, or to test a **Beta** version by switching the **Catalog channel** selector at the top of the Apps page.

## Turn your own program into a cartridge ROM

You can turn any Atari ST program into a cartridge ROM image with the [SidecarTridge USM web app](https://usm.sidecartridge.com). Drop a `.PRG` or `.TOS` file in your browser and download a ready-to-use 128 KB `.ROM` image. Everything runs locally in the browser, so there is nothing to install.

Copy the resulting `.ROM` file to the `/roms` folder of the microSD card and the [ROM Emulator microfirmware](/sidecartridge-multidevice/microfirmwares/rom_emulator/) will run it like a physical cartridge.

If you want to share your program with the community, you can also submit the resulting ROM to the [public ROM database](/sidecartridge-multidevice/publicromdb/).

## Wire external RESET and SELECT buttons

If you are building a custom enclosure and want to expose the RESET and SELECT buttons on the outside, boards from [revision 2.2.0](/sidecartridge-multidevice/revisions/#revision-220) onward provide two dedicated connectors for external buttons.

The connector specification, identical for both buttons:

- Family: **JST SH**
- Pin count: **2**
- Pitch: **1.0 mm**
- Board side: male header (SMT)
- Cable side: JST SH 2-pin 1.0 mm female housing with crimped contacts

The switch is a simple short between the two pins, so polarity does not matter. Any momentary tact switch wired to a JST SH 2-pin 1.0 mm female connector will work; pre-crimped cables are widely available from parts suppliers (housing `SHR-02V-S-B` with `SSH-003T-P0.2` contacts, or compatible).

We also sell a [wired pushbutton with the right connector already crimped](https://store.sidecartridge.com/products/push-button-switch-with-jst-sh-2x-1mm-female-connectors): momentary action, 7 mm mounting hole, nut and washer included.

Earlier board revisions (2.0 and 2.1) used a different button arrangement, so this section does not apply to them.

