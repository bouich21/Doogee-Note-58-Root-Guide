# Doogee-Note-58-Root-Guide
A complete dogee note 58 bootloader unlock guide 
Ok now before everything make sure you backup 
# Requirements:
A PC : especially windows but if you have problems with drivers go with Linux + Wine 
A USB cable : You can't make it wiresly 
Patience: May god have mercy 
# How to actually do it 
Ok so now we're gonna need the bug called CVE-2022-38694 it's low exploit bug that it patches with the Bootrom 
you'll need to download spd_flash from GitHub https://github.com/TomKing062/CVE-2022-38694_unlock_bootloader
and compile it 
than go to Releases and download ums_9230_universal_unlock_UFS ( the phone uses Umcp so it uses an ufs style flash ) 
# Really starting it 
Ok now poweroff your phone and start cmd with administrator 
CD to the directory that has spd_dump 
then unzip the files of ums_9320 ( I'm to lazy to write it again but you understood) then copy unlock_autopatch_9230.bat (it maybe be another filename idk ) and the fdl files 
then start the unlockautopatch it will say :
Waiting for dsl_connection 300s
now plug in your phone while holding power+voldown 
it will start executing commands 
then when it's done you might see a black screen this is completely normal it happens to me you just have to reflash the whole firmware 
if after the flash it rebooted 
# CONGRATULATIONS YOU UNLOCKED YOUR BOOTLOADER 
it's a pain I know since my sanity was destroyed after this 
# Disclaimer 
this method was tested using windows I tried using Linux but it tells you 
LIB_USB_TIMEOUT or connection closed or VAR expected 
REMEMBER IM NOT RESPONSIBLE FOR THE DAMAGE DONE TO THE PHONE YOU NEED TO KNOW IT WAS YOUR CHOICE 
if you plan on rooting it you need to use magisk because the kernel may be non gki 
# Good luck and may God be with you 
