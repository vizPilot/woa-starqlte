<img align="right" src="https://github.com/vicenteicc2008/woa-starqlte/blob/main/s9-woa.png" width="350" alt="Windows 11 running on starqlte">

# Running Windows on the Samsung Galaxy S9 SM-G9600

## Updating drivers

### Prerequisites
- [ADB & Fastboot](https://developer.android.com/studio/releases/platform-tools)

- [UEFI image](https://github.com/vicenteicc2008/woa-starqlte/releases/download/1.0-uefi/boot-starqlte.img)
  
- [Drivers](https://github.com/vicenteicc2008/woa-starqlte/releases/download/1.0-rc1-drivers/S9-Drivers.zip)
  
- [Msc script](https://github.com/vicenteicc2008/woa-starqlte/releases/download/1.0-rc1-msc/msc-s9.sh)
  
- [TWRP](https://github.com/vicenteicc2008/woa-starqlte/releases/download/1.0-rc1/twrp-starqltechn.tar) (should already be installed)

#### Boot to TWRP
> press Vol+ Key, Bixby Key and Power at the same time

#### Running the msc script
> Put msc.sh in the platform-tools folder, then run:
```cmd
adb push msc.sh / && adb shell sh msc.sh
```

### Diskpart
```cmd
diskpart
```

#### List device volumes
> To print a list of all the connected volumes, run
```cmd
list volume
```

#### Select Windows volume
> Replace $ with the actual number of **WINS9**
```cmd
select volume $
```

#### Assign letter to Windows
```cmd
assign letter x
```

#### Exit diskpart
```cmd
exit
```

### Installing Drivers
> Extract the drivers folder from the archive, then run the following command, replacing`<path\to\drivers>` with the actual path of the drivers folder
```cmd
dism /image:X:\ /add-driver /driver:<path\to\drivers> /recurse
```

### Unassign disk letter
> So that it doesn't stay there after disconnecting the device
```cmd
diskpart
```

#### Select the Windows volume of the phone
> Use `list volume` to find it, replace "$" with the actual number of **WINS9**
```diskpart
select volume $
```

#### Unassign the letter X
```diskpart
remove letter x
```

#### Exit diskpart
```diskpart
exit
```

##### Boot back into Windows
> Reboot your device to boot back into Windows.


## Finished!



















