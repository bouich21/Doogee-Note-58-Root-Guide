# Doogee Note 58 - Bootloader Unlock & Root Guide (Unisoc T7250)

A complete guide to unlocking the bootloader, rooting, and modifying the Doogee Note 58. 

> **BEFORE YOU START:** MAKE A BACKUP. Unlocking the bootloader will completely wipe your device. 
> **DISCLAIMER:** I AM NOT RESPONSABLE FOR THE DAMAGE YOUR GONNA CAUSE IT WAS YOUR DECISION TO FOLOW THIS GUIDE. My sanity was destroyed during this process, so maybe yours will be too. May god help you 

---

## Requirements

*   A Windows PC (Highly recommended. Linux works via Wine, but it constantly throws `LIBUSB_TIMEOUT` or `connection closed` errors. Windows is just easier for this).
*   A USB data cable (You cannot do this wirelessly).
*   The stock firmware `.pac` file (Extracted so you have `boot-gki.img` and `u-boot-spl-16k-ufs-sign.bin`) I'm gonna (or in the future was) to put the .oac firmware file on the release file.
*   **Patience.**: you need to have this (you can be angry when the phone don't want to do this just like I did)

### Files You Need to Download:
1.  **The Exploit Tool**: Download the compiled `spd_dump` for Windows from [TomKing062's GitHub](https://github.com/TomKing062/CVE-2022-38694_unlock_bootloader).
2.  **The Unlock Payload**: Download `ums9230_universal_unlock_UFS.zip` from the Releases page of that GitHub repo. *(Your phone uses uMCP storage, which uses the UFS flashing style).*
3.  **Magisk App**: To root the phone after unlocking.

---

##  Part 1: Unlocking the Bootloader

The Doogee Note 58 uses a Unisoc T7250 chip. There is no "guided unlock" for this phone. We have to use the BootROM exploit **CVE-2022-38694**.

1.  Create a folder on your PC and extract the `ums9230_universal_unlock_UFS.zip` files into it.
2.  Copy `spd_dump.exe`, `fdl1-dl.bin`, `fdl2-dl.bin`, and `unlock_autopatch_9230.bat` into that same folder.
3.  Turn off your phone **completely**.
4.  Open **Command Prompt (CMD) as Administrator** and `cd` into the directory where you put the files:
    ```cmd
    cd C:\path\to\your\unlock\folder
    ```
5.  Run the batch script:
    ```cmd
    unlock_autopatch_9230.bat
    ```
6.  The script will say: `Waiting for dl_diag connection (300s)`.
7.  **Plug in your phone while holding `Volume Down + Power`.** The screen will stay black. This is BROM mode.
8.  Let the script execute. It will back up your partitions and write the patched bootloader.

###  The Black Screen of Death
When the script finishes, your phone will reboot, but the screen will be **completely black**. **This is normal.** The patched bootloader doesn't initialize the display.

**How to fix the black screen (Flash stock bootloader):**
1.  Turn off the phone (hold Power for 15 seconds).
2.  Boot back into BROM mode (Hold `Vol Down + Power`, plug in USB).
3.  Run this exact command to flash the stock UFS bootloader back:
    ```cmd
    spd_dump --wait 300 exec_addr 0x65015f08 fdl fdl1-dl.bin 0x65000800 fdl fdl2-dl.bin 0x9efffe00 exec w splloader u-boot-spl-16k-ufs-sign.bin reset
    ```
4.  The phone will boot into Android. Go to `Settings > Developer Options > OEM Unlocking`. It should say **"Bootloader is already unlocked."**

#  CONGRATULATIONS, YOU UNLOCKED YOUR BOOTLOADER 

---

##  Part 2: Rooting with Magisk

The dogee may have a non gki kernel so it's adviced to use magisk 

1.  Extract `boot-gki.img` from your stock firmware.
2.  Copy `boot-gki.img` to your phone.
3.  Open the **Magisk** app on your phone, tap **Install**, then **"Select and Patch a File"**. Select `boot-gki.img`. 
4.  Magisk will create a `magisk_patched-xxxxx.img` file. Copy this file back to your PC.
5.  Turn off the phone and enter BROM mode again (`Vol Down + Power`, plug in USB).
6.  Flash the patched boot image to both slots using this command:
    ```cmd
    spd_dump --wait 300 exec_addr 0x65015f08 fdl fdl1-dl.bin 0x65000800 fdl fdl2-dl.bin 0x9efffe00 exec w boot_a magisk_patched-xxxxx.img w boot_b magisk_patched-xxxxx.img reset
    ```
7.  Reboot. Open Magisk,and you will have root (maybe)

---

##  Part 3: Customizing the UI (Optional)

If you want the iOS / Nothing OS look (custom clock, quick settings):
Since this is a Clean AOSP Android 15 build, do **NOT** use Vector or standard LSPosed. They will crash SystemUI with a `NoSuchFieldError`.

1.  Install **ReLSPosed v1.0.2 (Zygisk)** via Magisk. Ensure Magisk's built-in Zygisk is turned **OFF**.
2.  Install **Iconify v7.3.0** (compatible with Android 12-16).
3.  Enable Iconify inside ReLSPosed, reboot, and customize your lockscreen clock and QS panel.

---

##  Final Words
This method was tested using Windows. If you use Linux, you will likely get `LIBUSB_TIMEOUT`, `connection closed`, or `ver expected` errors because the Linux build lacks the `exec_addr` exploit. 

Good luck, and may God be with you. 
Also this guide was made by a 14 year old be grateful . My sanity is cooked 
