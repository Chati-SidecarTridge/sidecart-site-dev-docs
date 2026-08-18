---
layout: default
title: Migrating data from an old hard disk
nav_order: 4
nav_exclude: false
parent: ACSI2STM Hard Disk for Atari ST
---

# Migrating Data from an Old Hard Disk to the ACSI2STM
{: .no_toc }

Many people reach the ACSI2STM after years of running a real vintage hard disk on their Atari ST, such as an Atari SH-204/SH-205, a Megafile, or a third-party ACSI unit. A common goal is to move the files from that ageing drive onto the ACSI2STM before the old mechanism finally fails. This chapter explains how to approach that migration and, just as importantly, what to expect along the way.

<details open markdown="block">
  <summary>
    Table of contents
  </summary>
  {: .text-delta }
1. TOC
{:toc}
</details>

## Prerequisites: pick the device that fits the migration

The device you use has a large impact on how hard the migration gets. All three of our store options can do the job, but they do not carry the same amount of friction. Here they are in order, from the smoothest path to the most demanding.

1. **[SidecarTridge MultiDevice](https://sidecartridge.com/products/sidecartridge-multidevice-atari-st).** The easiest starting point. It uses a different port and a different technology from the ACSI bus, so it lets you copy data without fighting the physical problems of sharing the ACSI port with your old drive in the first place. You may still run into driver questions on the Atari side, but the bus level conflicts are gone from the outset.
2. **[ACSI2STM Compact](https://sidecartridge.com/products/acsi2stm-atari-st).** Same port as your old drive, different technology. You have to deal first with the problems of sharing the ACSI port and its daisy chain, and then with driver compatibility on top. It is built for chaining, so the physical side is manageable, but it asks more of you than the MultiDevice.
3. **[ACSI2STM Mini](https://sidecartridge.com/products/acsi2stm-mini-atari-st).** The same challenges as the Compact, plus two of its own. It is not meant to be chained on the ACSI bus, so it depends on the electronics of the other devices already on the bus behaving well. And changing its ACSI ID means soldering small pads, which is harder for anyone with limited soldering skills.

If you own more than one of these, reach for the one higher on this list before the one below it.

## Set your expectations first

Moving data from a 40 year old ACSI hard disk to a modern device is not a one click operation, and it is fair to say it is one of the more demanding tasks in the Atari ST hobby. You are dealing with two different storage devices on the same bus, each with its own driver and boot behaviour, plus the well known quirks of TOS on top. Plan the migration as a small project, not a quick fix, and give yourself time to work through it step by step.

{: .note}
This is a tricky area, and even experienced users hit friction here. For a candid, real world account of the same journey, an internal hard disk living alongside an ACSI2STM, see this write up: [ACSI2STM on a Mega STE](https://www.fplanque.com/tech/retro/atari/atari-st-acsi2stm-mega-ste/). The "driver hell" it describes is exactly why the SidecarTridge founder built the [SidecarTridge MultiDevice](/sidecartridge-multidevice/) and keeps maintaining tools such as [`atari-hd`](https://github.com/sidecartridge/atari-hd).

{: .warning}
If your Atari ST is heavily modified (non stock TOS, accelerators, alternative hard disk controllers, custom drivers), be aware that modified machines behave in less predictable ways. Some of those modifications ship with their own hard disk units and drivers, and those can interact with the migration in ways that are specific to your setup. Study your own machine before you change anything.

{: .warning}
Study the documentation in depth before you start this project. Do your homework and do not just read it, study it, because you are going to need it. We regularly meet users who lack not only the specific knowledge this migration requires but even the basic groundwork that a careful read of the documentation would have given them. Please also keep our scope in mind: we do not offer data migration services, and we are not an assistant you can query every time you get stuck. We provide support for the device, and that is where it ends.

## Choose your target format

Before copying anything, decide what the data should live on once it is on the ACSI2STM. There are two paths, and they are described in detail elsewhere in this documentation.

- **Legacy ACSI image.** This keeps you closest to the original setup, with a classic driver and partition layout on a disk image. Choose this if you depend on a specific driver or a bootable configuration. See [Creating images for the ACSI mode](/acsi2stm-atari-st/before-buy/#creating-images-for-the-acsi-mode) and the [`atari-hd`](https://github.com/sidecartridge/atari-hd) tool.
- **GEMDRIVE.** This is the modern route, where the microSD card is read directly as a FAT filesystem with no ACSI driver, larger volumes, and hot swap support. See [The GEMDRIVE protocol](/acsi2stm-atari-st/user-guide/#the-gemdrive-protocol) and [Formatting microSD cards for GEMDRIVE mode](/acsi2stm-atari-st/before-buy/#formatting-microsd-cards-for-gemdrive-mode).

For most people who simply want their files on modern storage, GEMDRIVE is the easier destination.

{: .warning}
GEMDRIVE was designed for easy access and storage, not for coexistence with old drive units. In our experience it is more reliable to copy from a legacy ACSI drive to a legacy mode ACSI image in the ACSI2STM unit. If GEMDRIVE works as an alternative with your unit and driver, go for it, but expect a bumpy ride. Older reports of GEMDRIVE being unusable alongside an old disk come from earlier firmware; newer firmware has worked in this scenario, so it is worth trying, but proceed with caution and verify every copy.

## Method 1: Copy files with both devices connected

This is the most common approach. You connect the old hard disk and the ACSI2STM at the same time, boot the machine so both are visible, and copy the files across from one to the other on the Atari itself.

1. **Give each device its own ACSI ID.** Two devices on the same bus must never share an ID. Old ACSI hard disks are usually fixed at ID 0, so leave the old disk where it is and shift the ACSI2STM out of the way with its ID_SHIFT pads. See [Multiple Devices and ACSI IDs](/acsi2stm-atari-st/user-guide/#multiple-devices-and-acsi-ids) and [Setting the ID_SHIFT](/acsi2stm-atari-st/user-guide/#setting-the-id_shift).
2. **Prepare the ACSI2STM target.** Have your destination ready before you boot, either a GEMDRIVE card or a fresh ACSI image, following the target format you chose above.
3. **Boot with both devices present.** The old disk boots its own driver from its boot sector and takes over drives C and so on as it always has. When an ACSI drive is also present on the bus, GEMDRIVE shifts its own drive letters to L and upwards to avoid clashing with the old driver. If GEMDRIVE does not appear, load its driver as described in [Mixing GemDrive with ACSI Drivers and Devices](/acsi2stm-atari-st/user-guide/#mixing-gemdrive-with-acsi-drivers-and-devices), typically by placing `GEMDRIVE.PRG` in the `AUTO` folder of the boot disk.
4. **Copy your files across.** With both sets of drive letters on the desktop, copy the files from the old disk (C, D) onto the ACSI2STM volume (for example L). Work in reasonable batches and let each copy finish before starting the next.
5. **Verify before you retire the old disk.** Open the copied files on the ACSI2STM side and confirm they are intact before you rely on them.

{: .note}
Getting both devices to mount at the same time is precisely the step where the driver conflicts show up. If the machine boots only one of the two, work out which driver each disk uses and its version, then in what order each one loads at boot. That is where the answer lives. There is no single setting that forces both to mount at once, so treat this as trial and error rather than a switch to flip.

## Method 2: Copy between two native ACSI drivers

Rather than mixing an old native driver with GEMDRIVE, set the ACSI2STM up with a legacy ACSI image driven by a native ACSI driver. You keep the old disk on its original native driver on one ID and the ACSI2STM on another ID, boot with both present, and copy the files across.

A very old driver such as ICD is often the safest first choice here, precisely because it is the kind of driver the original disks already used, so it is the most likely to read the old disk correctly. Modern drivers such as HDDRIVER or PPERA can also work, but they may not read a vintage disk reliably, so test carefully before you trust them with your only copy of the data.

This procedure works when two conditions are met. First, the driver that loads first must be able to understand the format of the second disk, so that a single driver can present both devices at once. Second, the ACSI IDs of the devices must be contiguous, with no gaps between them. Some drivers scan the whole bus regardless, but others stop as soon as they hit an ID with no disk on it, so a gap can leave the second device invisible.

The trade off compared with GEMDRIVE is more work up front: you prepare the image and install the driver on it before you start. See [Creating images for the ACSI mode](/acsi2stm-atari-st/before-buy/#creating-images-for-the-acsi-mode) and the [`atari-hd`](https://github.com/sidecartridge/atari-hd) tool.

## Method 3: Image the whole old disk (advanced)

If you have a way to read the old ACSI disk at block level on another system, you can capture a full image of it and run that image on the ACSI2STM in ACSI mode, preserving the exact original layout. This depends heavily on the hardware you have available to read a vintage ACSI drive, so it is beyond what this guide can cover generically. The community image and driver resources linked in [Creating images for the ACSI mode](/acsi2stm-atari-st/before-buy/#creating-images-for-the-acsi-mode) are a good starting point if you want to go this route.

## Method 4: Take a different route with the SidecarTridge MultiDevice

Getting two heterogeneous disks with different drivers and different formats to coexist on the same ACSI bus is often very hard. When that is the case, one option is to step off the ACSI bus altogether and use the [SidecarTridge MultiDevice](/sidecartridge-multidevice/). It creates virtual disks and carries its own implementation of GEMDRIVE that runs over the cartridge port rather than the ACSI bus, so it sidesteps the driver and bus conflicts entirely. That makes it a solid alternative when the migration on the ACSI side gets messy.

Keep in mind that moving data from one disk to another is not a flip of a switch but a properly planned and structured project. This is simply one more alternative to have in your toolbox.

## After the migration

- Once your data is on an SD card or in an image file, back it up on a PC. This is one of the biggest practical wins of moving off a mechanical drive: a copy of the whole volume is now a single file.
- Consider retiring the ageing mechanical disk for daily use and keeping it only as a reference, since its remaining lifespan is unknown.

If you get stuck at any step, the [Troubleshooting](/acsi2stm-atari-st/troubleshooting/) chapter and the [official ACSI2STM documentation](https://github.com/retro16/acsi2stm/tree/stable/doc) go deeper into driver behaviour and boot order.

[Previous: User Guide](/acsi2stm-atari-st/user-guide/){: .btn .mr-4 }
[Main](/acsi2stm-atari-st/){: .btn .mr-4 }
[Next: External LED](/acsi2stm-atari-st/external-led/){: .btn }
