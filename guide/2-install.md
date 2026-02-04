<img align="right" src="https://github.com/vicenteicc2008/woa-starqlte/blob/main/s9-woa.png" width="350" alt="Windows 11 running on starqlte">

# Running Windows on the Samsung Galaxy S9 SM-G9600

## Installing Windows

### Prerequisites
- [Windows on ARM image](https://worproject.com/esd)
  
- [Drivers](https://github.com/vicenteicc2008/woa-starqlte/releases/download/1.0-rc1-drivers/S9-Drivers.zip)

- [UEFI Image](https://github.com/vicenteicc2008/woa-starqlte/releases/tag/1.0-uefi)
  
- [Msc script](https://github.com/vicenteicc2008/woa-starqlte/releases/download/1.0-rc1-msc/msc-s9.sh)

- [Gdisk](https://github.com/vicenteicc2008/woa-starqlte/releases/download/1.0-rc1-msc/gdisk)

- [TWRP](https://github.com/vicenteicc2008/woa-starqlte/releases/download/1.0-rc1/twrp-starqltechn.tar) (should already be installed)

#### Boot to TWRP
> Disable MTP in TWRP

#### Running the msc script
> Put msc.sh in the platform-tools folder, then run:
```cmd
adb push msc.sh / && adb shell sh msc.sh
```

### Correct disk mapping in Windows disk manager 
https://sourceforge.net/projects/gptfdisk/files/gptfdisk/
https://mega.nz/file/K5UkQLrC#vCwlitpuZmEELGl33BxMMmv_NbGhJhaDWVoICm6sSAs

Here is instruction: use msc.sh then on PC in disk manager right click
on disk and click online(it will wipe primary gpt table in lun0). now in
recovery use "gdisk /dev/block/sda" (\\.\physicaldrive# where # your drive number in windows disk manager) and these commands "r, c, y, w
and y" it will restore gpt table, after reconnecting phone to pc it will show that disk is active.

### Diskpart
> [!WARNING]
> DO NOT ERASE, CREATE OR OTHERWISE MODIFY ANY PARTITION WHILE IN DISKPART!!!! THIS CAN ERASE ALL OF YOUR UFS!!!! THIS MEANS THAT YOUR DEVICE WILL BE PERMANENTLY BRICKED WITH NO SOLUTION! (except for taking the device to Samsung or flashing it with EDL (which doesn't even exist on Samsung devices), both of which will likely cost money)

```cmd
diskpart
```

#### Finding your phone
> This will list all connected disks
```cmd
lis dis
```

#### Selecting your phone
> Replace $ with the actual number of your phone (it should be the last one)
```cmd
sel dis $
```

#### Listing your phone's partitions
> This will list your device's partitions
```cmd
lis par
```

#### Selecting the Windows partition
> Replace $ with the partition number of Windows (should be 30)
```cmd
sel par $
```

#### Formatting Windows drive
```cmd
format quick fs=ntfs label="WINS9"
```

#### Add letter to Windows
```cmd
assign letter x
```

#### Selecting the ESP partition
> Replace $ with the partition number of ESP (should be 29)
```cmd
sel par $
```

#### Formatting ESP drive
```cmd
format quick fs=fat32 label="ESPS9"
```

#### Add letter to ESP
```cmd
assign letter y
```

#### Exit diskpart
```cmd
exit
```

### Installing Windows
> [!Warning]
> DO NOT USE 24H2!!!

> Replace `path\to\install.esd` with the actual path of install.esd (it may also be named install.wim)

```cmd
dism /apply-image /ImageFile:path\to\install.esd /index:6 /ApplyDir:X:\
```

> If you get `Error 87`, check the index of your image with `dism /get-imageinfo /ImageFile:path\to\install.esd`, then replace `index:6` with the actual index number of **Windows 11 Pro** in your image

### Removing Windows recovery and autocheck
> [!WARNING]
>
> If your phone enters Windows [recovery mode](https://learn.microsoft.com/en-us/windows-hardware/manufacture/desktop/windows-recovery-environment--windows-re--technical-reference?view=windows-11), it will damage your UFS and permanently brick the device!
>
> This issue only occurs on Samsung devices running Windows.
- Navigate to `X:\Windows\System32\Recovery\` and delete **WinRE.wim**.
- Navigate to `X:\Windows\System32`, find **autochk.exe**, and right click on it.
- Click on `Properties` > `Security` > `Advanced` > `Owner change` > **(Enter your PC username)**.
- Click `Add` > `Select a principal` > **(Enter your username)** > Check `"Full control"` under basic permissions.
- Now delete **autochk.exe** in `X:\Windows\System32\`.

### Installing Drivers
> Extract the drivers folder from the archive, then run the following command, replacing `path\to\drivers` with the actual path of the drivers folder
```cmd
dism /image:X:\ /add-driver /driver:path\to\drivers /recurse
```
  
#### Create Windows bootloader files
```cmd
bcdboot X:\Windows /s Y: /f UEFI
```

#### Enabling test signing
```cmd
bcdedit /store Y:\EFI\Microsoft\BOOT\BCD /set "{default}" testsigning on
```

#### Disabling recovery
```cmd
bcdedit /store Y:\EFI\Microsoft\BOOT\BCD /set "{default}" recoveryenabled no
```

#### Disabling integrity checks
```cmd
bcdedit /store Y:\EFI\Microsoft\BOOT\BCD /set "{default}" nointegritychecks on
```

#### Disabling boot status policy
```cmd
bcdedit /store Y:\EFI\Microsoft\BOOT\BCD /set "{default}" bootstatuspolicy IgnoreAllFailures
```

### Unassign disk letters
> So that they don't stay there after disconnecting the device
```cmd
diskpart
```

#### Select the ESP volume of the phone
> Use `list volume` to find it, replace "$" with the actual number of **ESPS9**
```diskpart
select volume $
```

#### Unassign the letter Y
```diskpart
remove letter y
```

#### Exit diskpart
```diskpart
exit
```

### Flashing UEFI
Flash UEFI on the modified recovery

### Reboot to Windows 11
> To set up your account in Windows 11

















