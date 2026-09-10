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
3. Copy the downloaded `.uf2` file corresponding to your Raspberry Pi Pico W into the `RPI-RP2` drive. The board flashes itself and reboots automatically.
4. Disconnect the USB cable. The new firmware has now been flashed.

## Format the microSD card

To use the Multi-device effectively, your microSD card needs to be formatted in FAT32 or exFAT. **We strongly recommend using a high-quality SDHC, SDXC or SDUC microSD from a reputable brand** to ensure optimal performance and reliability. To format the microSD card, you can use the [SD Card Formatter](https://www.sdcard.org/downloads/formatter/) tool available for PC/Mac/Linux.

{: .note }
Always ensure you've selected the correct device to format, especially when working with disk utilities, to avoid data loss.
{: .note }

