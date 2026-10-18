# Doogee Note 58 - Bootloader Unlock & Root Guide (Unisoc T7250)

A guide to unlocking the bootloader, rooting, and customizing the Doogee Note 58.

**TL;DR:** You use a BootROM exploit (CVE-2022-38694) from a Windows PC to unlock the bootloader, then patch the stock boot image with Magisk and flash it back. Unlocking **wipes your phone**.

> **BEFORE YOU START:** MAKE A BACKUP. Unlocking the bootloader will completely wipe your device.
>
> **DISCLAIMER:** I am not responsible for any damage. Following this guide was your decision. My sanity was destroyed during this process, so maybe yours will be too. May God help you.

---

## Requirements

- A **Windows PC** (highly recommended). Linux via Wine constantly throws `LIBUSB_TIMEOUT` or `connection closed` errors, so Windows is just easier for this.
- A **USB data cable** (you can't do this wirelessly).
- The **stock firmware `.pac` file** for your phone, from Doogee's official download page. Extract it so you have `boot-gki.img` and `u-boot-spl-16k-ufs-sign.bin`.
- **Patience.** You will get angry when the phone doesn't cooperate. I did too.

### Files to download

1. **The exploit tool:** the compiled `spd_dump` for Windows from [TomKing062's GitHub](https://github.com/TomKing062/CVE-2022-38694_unlock_bootloader).
2. **The unlock payload:** `ums9230_universal_unlock_UFS.zip` from the Releases page of that same repo. (This phone uses uMCP storage, which uses the UFS flashing style.)
3. **The Magisk app:** to root the phone after unlocking.

Credit for the exploit goes to TomKing062. I only wrote down how I used it on this phone.

---

## Part 1: Unlocking the Bootloader

The Doogee Note 58 uses a Unisoc T7250 chip. There is no "guided unlock" for this phone, so we use the BootROM exploit **CVE-2022-38694**.

1. Create a folder on your PC and extract `ums9230_universal_unlock_UFS.zip` into it.
2. Copy `spd_dump.exe`, `fdl1-dl.bin`, `fdl2-dl.bin`, and `unlock_autopatch_9230.bat` into that same folder.
3. Turn off your phone **completely**.
4. Open **Command Prompt (CMD) as Administrator** and go to the folder:
   ```cmd
   cd C:\path\to\your\unlock\folder
   ```
5. Run the batch script:
   ```cmd
   unlock_autopatch_9230.bat
   ```
6. The script will say: `Waiting for dl_diag connection (300s)`.
7. **Plug in your phone while holding `Volume Down + Power`.** The screen stays black. This is BROM mode.
8. Let the script run. It backs up your partitions and writes the patched bootloader.

### The Black Screen of Death

When the script finishes, your phone will reboot, but the screen will be **completely black**. **This is normal.** The patched bootloader doesn't initialize the display.

**Fix: flash the stock bootloader back**

1. Turn off the phone (hold Power for 15 seconds).
2. Enter BROM mode again (hold `Vol Down + Power`, plug in USB).
3. Run this exact command to flash the stock UFS bootloader:
   ```cmd
   spd_dump --wait 300 exec_addr 0x65015f08 fdl fdl1-dl.bin 0x65000800 fdl fdl2-dl.bin 0x9efffe00 exec w splloader u-boot-spl-16k-ufs-sign.bin reset
   ```
4. The phone boots into Android. Go to `Settings > Developer Options > OEM Unlocking`. It should say **"Bootloader is already unlocked."**

# CONGRATULATIONS, YOU UNLOCKED YOUR BOOTLOADER

---

## Part 2: Rooting with Magisk

This phone may use a non-GKI kernel, so Magisk is the recommended root method here.

1. Extract `boot-gki.img` from your stock firmware. **Keep an untouched copy**, because you may need it to recover.
2. Copy `boot-gki.img` to your phone.
3. Open the **Magisk** app, tap **Install**, then **"Select and Patch a File"**, and select `boot-gki.img`.
4. Magisk creates a `magisk_patched-xxxxx.img` file. Copy it back to your PC.
5. Turn off the phone and enter BROM mode again (`Vol Down + Power`, plug in USB).
6. Flash the patched boot image to both slots:
   ```cmd
   spd_dump --wait 300 exec_addr 0x65015f08 fdl fdl1-dl.bin 0x65000800 fdl fdl2-dl.bin 0x9efffe00 exec w boot_a magisk_patched-xxxxx.img w boot_b magisk_patched-xxxxx.img reset
   ```
7. Reboot and open Magisk. You should now have root.

**If it doesn't work (bootloop or no root):** make sure the `boot-gki.img` you patched came from the **same firmware version** that's on your phone. To undo, repeat step 6 with the original `boot-gki.img` instead of the patched one.

---

## Part 3: Customizing the UI (Optional)

If you want the iOS / Nothing OS look (custom clock, quick settings): this is a clean AOSP Android 15 build, so do **NOT** use Vector or standard LSPosed. They crash SystemUI with a `NoSuchFieldError`.

1. Install **ReLSPosed v1.0.2 (Zygisk)** via Magisk. Make sure Magisk's built-in Zygisk is turned **OFF**.
2. Install **Iconify v7.3.0** (compatible with Android 12-16).
3. Enable Iconify inside ReLSPosed, reboot, and customize your lockscreen clock and QS panel.

---

## Troubleshooting

| Problem | Things to try |
|---|---|
| Script stuck on `Waiting for dl_diag connection` | Use a real data cable, try a different USB port (a USB 2.0 port on the back of the PC is best), and make sure the Unisoc/SPD USB driver is installed. |
| `LIBUSB_TIMEOUT` / `connection closed` | Use Windows instead of Linux/Wine, then power off completely and retry BROM mode. |
| `ver expected` | Linux build issue. Use the Windows build. |
| Black screen after unlock | Normal. See "The Black Screen of Death" above. |
| Bootloop after rooting | Flash your original `boot-gki.img` back to both slots (Part 2, step 6). |

---

## Final Words

This guide was tested on **Windows only**. On Linux you will likely get `LIBUSB_TIMEOUT`, `connection closed`, or `ver expected` errors, and the Linux build may lack the `exec_addr` support this method needs, so I don't recommend it.

Good luck, and may God be with you.be grateful I'm 14
