<img align="right" src="https://github.com/vicenteicc2008/woa-starqlte/blob/main/s9-woa.png" width="350" alt="Windows 11 running on starqlte">

# Running Windows on the Samsung Galaxy S9 SM-G9600

## Partitioning your device

### Prerequisites
- A brain (most important of all)

- [ADB & Fastboot](https://developer.android.com/studio/releases/platform-tools)
  
- [Modded TWRP](https://github.com/vicenteicc2008/woa-starqlte/releases/download/1.0-rc1/twrp-starqltechn.tar)

- [Parted](https://github.com/n00b69/woa-beryllium/releases/download/Files/parted)

### Notes
> [!WARNING]  
> 
> DO NOT REBOOT YOUR PHONE! If you think you made a mistake, ask for help in the [Discord server](https://discord.gg/eXrYQdvVq8).
> 
> Do not run all commands at once, execute them in order!
>
> YOU CAN BREAK YOUR DEVICE WITH THE COMMANDS BELOW IF YOU DO THEM WRONG!!!

### Opening CMD as an admin
> Download **platform-tools** and extract the folder somewhere, then open CMD as an **administrator**.
>
> It is recommended to keep this window open and use it throughout the entire guide.
> 
> Replace `path\to\platform-tools` with the actual path to the platform-tools folder, for example **C:\platform-tools**.
```cmd
cd path\to\platform-tools
```

#### Flash the modded TWRP with Odin

#### Backing up important files
> This will back up **fsc**, **fsg**, **modemst1** and **modemst2** to the current path your CMD is opened in (for example **C:\platform-tools**). Confirm these files are actually there before proceeding.
>
> If you've got anything else you want to back up, do this now. Your Android data will be erased in the next steps.
```cmd
cmd /c "for %i in (fsg,fsc,modemst1,modemst2) do (adb shell dd if=/dev/block/by-name/%i of=/tmp/%i.bin & adb pull /tmp/%i.bin)"
```

### Partitioning guide
> Your Samsung Galaxy S9 SM-G9600 may have different storage sizes. This guide uses the values of the 128GB model as an example. When relevant, the guide will mention if other values can or should be used.

#### Unmount data
- Go to "Mount" in TWRP and unmount data, if it is mounted

#### Preparing for partitioning
> Download the parted file and move it in the platform-tools folder, then run
```cmd
adb push parted /cache/ && adb shell "chmod 755 /cache/parted" && adb shell /cache/parted /dev/block/sda
```

#### Printing the current partition table
> Parted will print the list of partitions, userdata should be the last partition in the list.
```cmd
print
```

#### Removing userdata
> Replace **$** with the number of the **userdata** partition, which should be **29**
```cmd
rm $
```

#### Creating ESP partition
> Replace **1611MB** with the former start value of **userdata** which we just deleted
>
> Replace **32GB** with the end value you want **userdata** to have
```cmd
mkpart esp fat32 1611MB 1911MB
```

#### Creating Windows partition
> Replace **32.3GB** with the end value of **esp**
>
> Replace **123GB** with the end value of your disk, use `p free` to find it
```cmd
mkpart win ntfs 32.3GB 123GB
```

#### Making ESP bootable
> Use `print` to see all partitions. Replace "$" with your ESP partition number, which should be 29
```cmd
set $ esp on
```

#### Exit parted
```cmd
quit
```

## [Next step: Installing Windows](/guide/2-install.md)





















