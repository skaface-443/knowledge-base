## ⚠ Recent news/issues
- Check our [latest news](https://github.com/starcitizen-lug/knowledge-base/wiki#news) for known temporary issues, workarounds, and runner/dxvk/driver requirements (especially Nvidia users!)

## Troubleshooting Steps

#### First things to try
1. Make sure our [LUG Helper](https://github.com/starcitizen-lug/lug-helper)'s Preflight Check passes all checks.
2. Make sure all prerequisites from the [Quick Start Guide](Quick-Start-Guide) are satisfied on your system.
3. Kill all wine processes and re-launch a fresh instance of the game.
   Navigate to `~/Games/star-citizen` and run the following in your terminal `./sc-launch.sh shell` then `wineserver -k`
5. Look for your issue in the [latest news](https://github.com/starcitizen-lug/knowledge-base/wiki#news)
6. Use the lug helper to get the latest wine runner. Be sure to check the [latest news](https://github.com/starcitizen-lug/knowledge-base/wiki#general-news) for recommendations
7. Try a different wine version. If using wine-staging, try standard wine. Try wine-staging if using standard wine.
8. Look for your issue/error in the categories on this page. Refer to the steps directly below to gather logs.

#### Gathering logs
- Wine log: `~/Games/star-citizen/sc-launch.log`
- Launcher log: `~/Games/star-citizen/drive_c/users/$USER/AppData/Roaming/rsilauncher/logs/log.log`
- Game log: `~/Games/star-citizen/drive_c/Program Files/Roberts Space Industries/StarCitizen/LIVE/Game.log`
- Native Lutris `lutris -d > ~/lutrislog.log`
- Flatpak Lutris `flatpak run net.lutris.Lutris -d > ~/lutrislog.log`


#### Community Help
If this page doesn't help resolve your issue, you may ask for help on our [social channels](https://github.com/starcitizen-lug/knowledge-base/wiki#welcome-space-penguins)


## Contents
💾 [Install & Update Problems](#-install--update-problems)  
💥 [Crashes](#-crashes)  
🧊 [Freezes](#-freezes)  
🤪 [Unexpected Behavior (sometimes also crashes)](#-unexpected-behavior-sometimes-also-crashes)  
💚 [Nvidia](#-nvidia)  
❤️ [AMD](#-amd)  
🕹️ [Controller Issues](#-controller-issues)  
🦦 [Lutris Issues](#-lutris-issues)  
👾 [32bit Drivers](#-32bit-drivers)  
❔ [Other Issues](#-other-issues)  

***



## 💾 Install & Update Problems

#### General troubleshooting steps
- Refer to [First things to try](#first-things-to-try) above.
- Make sure you are not trying to install to an NTFS-formatted drive.
- Be sure you haven't changed the default install path in the RSI Launcher settings. If you wish to install the game elsewhere, put the entire wine prefix there instead.


#### Launcher installation hangs at Updating Game Content
- The Launcher sometimes hangs during this phase of the install process
- Completely quit the launcher, ensuring no lingering wine processes remain, then verify files


#### Wine install fails to create .desktop files
- Manually create a file `~/.local/share/applications/RSI Launcher.desktop` based on the following template.
- Edit paths based on your install, then update the cache by running `update-desktop-database ~/.local/share/applications`
  ```
  [Desktop Entry]
  Name=RSI Launcher
  Exec="/home/{user}/Games/StarCitizen/sc-launch.sh"
  Type=Application
  StartupNotify=true
  Comment=RSI Launcher
  Path=/home/{user}/Games/StarCitizen/dosdevices/c:/Program\sFiles/Roberts\sSpace\sIndustries/RSI\sLauncher
  Icon=rsi-launcher.png
  StartupWMClass=rsi launcher.exe
  ```

#### RSI Launcher Error Code 2000
- Follow [Launcher Tips](Tips-and-Tricks#rsi-launcher-20) instructions


#### RSI Launcher executable is missing
- Automatic update sometimes fails or is interrupted
- Follow manual update [instructions](Tips-and-Tricks#rsi-launcher-manual-update)

#### RSI Launcher doesn't auto-update
- Follow manual update [instructions](Tips-and-Tricks#rsi-launcher-manual-update)

#### RSI Launcher error: *Could not fix permissions on directory*
- Please follow our [step-by-step guide](Tips-and-Tricks#rsi-launcher-20) for the 2.0 RSI Launcher

#### Installing Star Citizen on an NTFS-formatted drive
- Don't; it probably won't work and will likely only corrupt your game files.

#### Popup warning 64-bit Windows is required
- Ensure that you are using a [Recommended](Tips-and-Tricks#recommended-distros) 64 bit linux distro
- Override environment variable WINEARCH=64

***




## 💥 Crashes

#### Error Code 210 and #1 Crash
- Standard Wine versions >10.1 made changes that break Easy Anti-Cheat
- Use the Helper to switch to a [recommended](Tips-and-Tricks#recommended-runners) wine runner


#### Code 3 crash with error: *Star Citizen process exited abnormally (code: 3) : Command failed*
- EAC bootstrapper failed to start up
- You may be missing 32bit drivers. See [32bit Drivers](#-32bit-drivers) below for more information
- Additionally, explicitly set the DXVK device name
  - identify device name using command `vulkaninfo --summary | grep deviceName`
  - set device name with environment variable `DXVK_FILTER_DEVICE_NAME=yourdevicenamehere`
- If using a flatpak launcher, you may need to resync it with your system. Run: `flatpak update`


#### Game immediately crashes after clicking 'Launch'
- Start by checking the Wine output and/or "game.log" file

- See [latest news](https://github.com/starcitizen-lug/knowledge-base/wiki#general-news) for information on recent stability issues

- Possible cause: DXVK
  - Make sure DXVK is installed and enabled
  - Nvidia users, check our [latest news](https://github.com/starcitizen-lug/knowledge-base/wiki#news) and Nvidia troubleshooting section [below](#-nvidia) for gpu driver issues, necessary workarounds, and currently recommended runner/DXVK versions.

- Possible cause: Incorrect Vulkan device
  - If you have Intel integrated graphics and see `VK_ICD_FILENAMES="/usr/share/vulkan/icd.d/intel_hasvk_icd.x86_64.json` in your log, then change the Vulkan device to use your discrete GPU:
    - identify device name using command `vulkaninfo --summary | grep deviceName`
    - set device name with environment variable `DXVK_FILTER_DEVICE_NAME=yourdevicenamehere`

- Possible cause: GPU drivers not working properly
  - If, in your "game.log", you see only `llvmpipe` listed as the working video adapter, your gpu drivers may not be functioning properly.

- Possible cause: Out of date flatpak libraries/packages
  - If you are using flatpak and have not run `flatpak update` recently, this should be done regularly to keep everything up to date.

- Possible cause: Phantom joystick
  - Many keyboards and mice can also have a "joystick" part in Linux, which Wine can detect. Unfortunately, Wine may be confused about it, as they are not real joysticks. In this case, the game could crash. Use the LUG Helper's maintenance menu to run the wine control panel and disable your keyboard and/or mice.


#### Game crashes after clicking 'Verify'
- Make sure Star Citizen is installed on drive "C:\" Check the "Library Folder" option in the launcher settings:
![Star Citizen launcher](https://media.discordapp.net/attachments/608349808956276737/927652866389340310/Screenshot_from_2022-01-03_14-56-37.png)
- Additionally, make sure the wine prefix is not installed on an NTFS formatted partition.oh keep in mind might be interesting to see what the model produces


#### Game crashes with "create_view: Assertion `!((UINT_PTR)base & page_mask)' failed" / "00adntdll:FILE_GetNtStatus Converting errno 12 to STATUS_UNSUCCESSFUL"
- Make sure you have set your vm.max_map_count as described in the installation section.


#### Crash or black screen while using Vulkan beta renderer
- Check the [latest news](https://github.com/starcitizen-lug/knowledge-base/wiki#news) recommendations for your graphics card
- Go back to DX11 by using the Delete Shaders option in the RSI Launcher > Settings > Games > {LIVE,PTU} > Delete  Local Settings > Shaders folder


#### Failed to decompress file/corrupted block detected error
- Some Penguins have had this error when using BTRFS. We suspect a regression of some kind.
- Easiest: Just ignore the error and continue playing. It doesn't crash until you click the button.
- Or fix it: Disable CoW on the game's StarCitizen dir
  1. Navigate to the Game folder `star-citizen/drive_c/Program Files/Roberts Space Industries`
  2. Use a terminal to run the command ```chattr +C ./StarCitizen```, then redownload or copy in a fresh data.p4k.
- Less easy Fix: Disable CoW only on the data.p4k file. This can only be done on an empty file.
- Alternatively, try mounting with the `compress` option instead of `compress-force`.
- If that doesn't work, switching to ext4 is an option.


#### After playing for a while, crash with no errors
  - If there are no errors in your [game logs](#gathering-logs), check your system logs. It may be an Out Of Memory situation. Create a larger [swap file](Performance-Tuning#zram--swap).


***




## 🧊 Freezes

#### Game hangs at splash screen or black/transparent window after clicking 'Launch'
- Make sure DXVK is installed. Use the [LUG Helper](https://github.com/starcitizen-lug/lug-helper) to install it
- Nvidia users, check our [latest news](https://github.com/starcitizen-lug/knowledge-base/wiki#news) for gpu driver issues, necessary workarounds, and currently recommended runner/DXVK versions.


#### Launcher freezes within a few seconds of opening
- Try using a different wine runner


***


## 🤪 Unexpected Behavior (sometimes also crashes)

#### Mouse/Cursor warp issues and view snapping in interaction mode
*This issue can also manifest as some main menu buttons not working due to the cursor actually being offset*

Switch to the game's software cursor. Create a user.cfg file in the LIVE, PTU, EPTU, TECH-PREVIEW directory with the following contents:
 ```
   #use software cursor
   pl_pit.forceSoftwareCursor = 1
 ```
Alternatively, you may choose Xorg at your login screen instead of Wayland session.
Other potential workarounds:
- Non-staging Wine Wayland helps mitigate this for some
  - Add environment variable `DISPLAY=` to unset it to empty
  - Add RSI Launcher.exe argument ` --in-process-gpu`
- [Proton](https://github.com/starcitizen-lug/knowledge-base/wiki/Alternative-Installations#proton-installation) helps mitigate this for some
- Gamescope helps mitigate this for some
  - **Note for Nvidia users:** Gamescope may not work on your hardware. See [a possible fix below](#gamescope-not-working)
  - Install and enable gamescope. Set these options for your display resolution `-W 2560 -H 1440 --force-grab-cursor`
  - Other Gamescope settings that may be required depending on your system: `Window Mode` set to `Fullscreen` if it doesn't launch fullscreen, `-g` in `Custom Settings` to grab keyboard

- Switching to an alternate desktop environment may help; most Gnome users don't seem to experience this issue
- You may try building xwayland with [this patch](https://github.com/Nobara-Project/rpm-sources/blob/main/baseos/xorg-x11-server-Xwayland/xwayland-pointer-warp-fix.patch) applied. If using KDE and patching xwayland, you will also need to install Gamescope and use the `--force-grab-cursor` option


#### Mouse/Cursor restricted to a region smaller than the display, or clicks offset from cursor
- Create a user.cfg file in the LIVE, PTU, EPTU, TECH-PREVIEW directory
 ```
 #set to your display resolution
 r_width = 3440
 r_height = 1440
 ```
- Hyprland users: Try using the in-game fullscreen option instead of the Hyprland equivalent.


#### Empty launcher
- Log out log back in, or reset the launcher by pressing Ctrl+Shift+Alt+R


#### Visual glitches or semi-transparent lines, poor performance, possibly random crashes
- DXVK may be broken or disabled. Reinstall it using the LUG Helper DXVK and maintenance menus
- Your game shader cache may need to be cleared. Use the Delete Shaders option in the RSI Launcher > Settings > Games > {LIVE,PTU} > Delete Local Settings > Shaders folder


#### Account login failed (possible code 19000)
This is a generic error code representing any issue with logging in to CIG servers
- Expected when attempting launch/login during a patch release
- Kill the launcher and restart it and try again
- Ensure that IPv6 is not disabled

#### Anticheat encountered an error (possible code 30033, 30034)
- Please follow [our EAC migration instructions](Tips-and-Tricks#easy-anti-cheat)
- Check your process list for any lingering wine processes. Reboot if necessary.
- You may have to delete the EAC directory in youre prefix's `AppData/Roaming` directory.
- You may also have to delete the EAC directory in the Star Citizen `LIVE` directory, followed by verifying files in the launcher.


#### Required Vulkan Extensions are missing error / poor performance compared to windows / error code 3
- Check if you have amdvlk installed by running `vulkaninfo --summary`. The vulkaninfo utility is part of the package `vulkan-tools` on most distros. You can also check your package manager.
- If your system is using amdvlk, uninstall that package and replace it with `vulkan-radeon`.
- For additional help with this, ask in our [Discord](https://github.com/starcitizen-lug/knowledge-base/wiki#welcome-space-penguins) tech support channel.


#### DirectX error message
- Error may read "Star Citizen requires DirectX feature level of 11.1 as a minimum which is not supported at present on this machine"  
  ![image](https://user-images.githubusercontent.com/3657071/224719841-ba1e831b-4ace-4f14-b423-3e49528154c6.png)
- Check that the `Vulkan device` is not set to an integrated gpu (ie, Intel)
  - identify device name using command `vulkaninfo --summary | grep deviceName`
  - set device name with environment variable `DXVK_FILTER_DEVICE_NAME=yourdevicenamehere`
  - verify by setting environment variable `DXVK_HUD=1` and observing the device name in the upper left of the screen
- Also make sure your GPU drivers (Mesa/nvidia) are up to date and DXVK is enabled/updated.
  - Use the LUG Helper to update dxvk
- Try switching to a non-staging wine runner from our [recommended runners](Tips-and-Tricks#recommended-runners) list
- Try [downgrading](Tips-and-Tricks#updating-dxvk-within-a-wine-prefix) to an older DXVK version


#### Failed to initialize dependencies error
- Make sure the `SDL_VIDEODRIVER` environment variable is **NOT** set globally to `wayland` on your system: `env | grep SDL_VIDEODRIVER`
- If it is set, remove it from whichever environment config(s) is setting it.


#### Black or flickering window, possible crash with errors 15006 or 30007
- Check for larger resolutions and scaling settings.  See CIG's [support article](https://support.robertsspaceindustries.com/hc/en-us/articles/360000081887-Guide-to-Graphic-Issues#large-res)


#### Black/transparent window after clicking 'Launch'
- Check our [latest news](https://github.com/starcitizen-lug/knowledge-base/wiki#news) for gpu driver issues, necessary workarounds, and currently recommended runner/DXVK versions.
- Make sure DXVK is installed and enabled


#### Non-US keyboard keys not working
- Set the `LANG=` environment variable. For example, `LANG=de_DE`
- Check the [latest news](https://github.com/starcitizen-lug/knowledge-base/wiki#general-news) and use the LUG Helper "manage runners" options to select a recent **staging** wine
- Use the LUG Helper Maintenance menu `Open Wine prefix configuration` button to run winecfg
- Select your language from the list and enable scancode auto-detection
    ![staging_input_menu](https://github.com/user-attachments/assets/a525f310-3a2a-49b1-bb6e-c07875e15608)


***




## 💚 Nvidia

#### Current known issues
- See the Nvidia section of our [latest news](https://github.com/starcitizen-lug/knowledge-base/wiki#news) for the ways in which Nvidia is being special today.

#### Crash when taking shield damage in-game
- There is a shield rendering bug that causes the game to crash. It seems to affect 1000 series cards.
- There is currently no known workaround other than switching cards. We recommend AMD.

#### Popup saying your Nvidia graphics driver is out of date
Typically caused by dxvk being broken or not installed
- Use the LUG Helper to update dxvk

#### Game fails to start after clicking Launch Game on laptops with Nvidia GPU + intel graphics
- Errors may include `DXVAVDA fatal error: could not LoadLibrary: msvproc.dll` or `Major opcode of failed request:  156 (NV-GLX)`
- Try removing all optimus/prime env vars for render offload and set the Vulkan device to the Nvidia GPU.
  - identify device name using command `vulkaninfo --summary | grep deviceName`
  - set device name with environment variable `DXVK_FILTER_DEVICE_NAME=yourdevicenamehere`

#### Severe frame drops
- Some Penguins are seeing VRAM exhaustion problems on nvidia cards
- Use an environment variable to override the max device memory. Refer to [DXVK config](https://github.com/doitsujin/dxvk/blob/master/dxvk.conf) for examples
  - Add a new `DXVK_CONFIG` environment variable
    - Card with 12GB vram: `export DXVK_CONFIG="dxgi.maxDeviceMemory = 9216;cachedDynamicResources = a;"`
    - Card with 10GB vram: `export DXVK_CONFIG="dxgi.maxDeviceMemory = 8192;cachedDynamicResources = a;"`
    - Card with  8GB vram: `export DXVK_CONFIG="dxgi.maxDeviceMemory = 6144;cachedDynamicResources = a;"`
    - Card with  6GB vram: `export DXVK_CONFIG="dxgi.maxDeviceMemory = 4096;cachedDynamicResources = a;"`
    - Card with  4GB vram: `export DXVK_CONFIG="dxgi.maxDeviceMemory = 2048;cachedDynamicResources = a;"`


#### DLSS (Deep Learning Super Sampling)
1. Use a standard (non-staging) Wine runner. There is a memory allocation issue with libcuda + wine-staging and Easy Anti-Cheat makes this prohibitively difficult to overcome.
2. Use winetricks `20250102-next` or newer to install `dxvk` and `dxvk_nvapi`. Replace the WINEPREFIX path with your game location and run:
   1. `WINEPREFIX=/home/{user}/Games/StarCitizen winetricks -f dxvk dxvk_nvapi`
3. Enable NVAPI with the following environment variable. This can added to `sc-launch.sh` for Wine installs via the Helper by selecting the edit the launch script option in its Maintenance menu.
   1. `DXVK_ENABLE_NVAPI=1`
4. Copy nvngx dlls provided by your nvidia driver into your wine prefix's `system32` folder:
   1. Locate the following dlls in `/usr/lib/nvidia/wine/` or `/usr/lib64/nvidia/wine`: `_nvngx.dll`, `nvngx.dll`, and `nvngx_dlssg.dll`
   2. Copy all three to your wine prefix's `system32` folder. On a default install, that will be `/home/{user}/Games/StarCitizen/drive_c/windows/system32`
5. Define the nvngx dlls in your prefix's registry:
   1. `WINEPREFIX=/home/{user}/Games/StarCitizen wine reg add "HKLM\\Software\\NVIDIA Corporation\\Global\\NGXCore" /v "FullPath" /t REG_SZ /d "C:\\Windows\\System32" /f`
6. Create the fake DLLS that the game requires to exist:
   1. Navigate inside your wine prefix to the system32 directory. On a default install, that will be `/home/{user}/Games/StarCitizen/drive_c/windows/system32`
   2. Duplicate any existing dll 3 times, for example `acledit.dll`, and rename the copies to these three filenames: `cryptbase.dll`, `devobj.dll`, and `drvstore.dll`


#### Gamescope not working
- See this [known issue report](https://github.com/ValveSoftware/gamescope/issues/526).
- A possible fix is to set the environment variable `__GL_THREADED_OPTIMIZATIONS=0`


***




## 💖 AMD

#### Current known issues
- See the AMD section of our [latest news](https://github.com/starcitizen-lug/knowledge-base/wiki#news)

#### Vulkan Beta: Bright flickering lights at edges of in-game display panels
- To fix: Add environment variable `radv_zero_vram=true`

***



## 🕹 Controller Issues

- See our sticks, throttles, and pedals [Troubleshooting Page](Sticks,-Throttles,-&-Pedals#troubleshooting)

***



## 🦦 Lutris Issues

#### Lutris General troubleshooting steps
- In Lutris, try setting `Prefer system libraries` to `On` globally before installation. After installation, this can be reset and configured only for Star Citizen if desired.
- Kill all wine processes and re-launch a fresh instance of the game. Select the game, click the arrow beside the wine button, choose `Open Bash terminal` and run `wineserver -k` and then restart

#### In Lutris, right clicking on Star Citizen and selecting "Configure" does not bring up the configuration
- Completely close Lutris with `kill lutris`, delete everything inside the Lutris cache directory `~/.cache/lutris`, and relaunch Lutris.


#### Launcher hangs / stops responding / crashes with an "ASAR" error
- Some people report the launcher hanging in combination with the Lutris runtime. If you are on Lutris, try toggling "Disable Lutris runtime" under "System options" of the Lutris game options.


#### RSI Launcher v1.6.2+ JavaScript error
- The LUG Helper's [wine installation](Quick-Start-Guide#installation-steps) method is recommended and avoids this error
- In the game's Lutris `System options`, make sure Advanced options is toggled on then in the `text-based games` section enable `CLI mode`
- Alternatively, select a Proton runner in Lutris' runner options tab, i.e. `ge-proton` and follow our [proton setup instructions](Tips-and-Tricks#proton)


#### Library version errors during installation
- If using a rolling release or bleeding edge distro, try toggling `Prefer system libraries` in Lutris to `On`.


#### Installer Error Code 256 / Downloading [exe file] failed (ie arial32.exe)
- Try setting `Prefer system libraries` to `On` in global lutris options.
- Inspect install log for failed winetricks downloads or sha256 mismatch, note the URL of the files being downloaded and its destination in winetricks' cache.
- Download each file manually to its destination in winetricks' cache.
- Use winetricks to ensure that the prefix is set to Win10 mode.


#### Lutris error: *Command exited with code 512*
- We suspect this is a Lutris bug and fixes seem inconsistent.
- Try going to Lutris Preferences -> Sources, and toggle either `Lutris` or `Local` and then toggle it back again.
- If you have an old install, try importing that into Lutris.


#### Lutris error: *"Runtime Error('No path can be generated for DXVK because no version information is available.')"*
- If you've just installed Lutris, be sure to launch it once, separately from the Star Citizen install process, to fully populate its runtime and caches.


#### RSI Launcher white screen / error DCompositionCreateDevice
- In Lutris, configure Star Citizen (right-click->Configure->Game options) and add `"--use-gl=osmesa"` to the Arguments field.
- If launching manually: `wine "RSI Launcher.exe" "--use-gl=osmesa"`


#### RSI Launcher fails to launch from lutris with CLI mode enabled
- Error message may be similar to `/usr/bin/gnome-terminal.real: symbol lookup error: /lib/x86_64-linux-gnu/libatk-bridge-2.0.so.0: undefined symbol: atk_component_scroll_to`
- Disable the lutris runtime in lutris global preferences to work around this error


#### No sound in game
- Depending on your distribution, you may need to set `Prefer System Libraries` in Lutris to `ON`.
- If you have sound in the launcher but not in the game, launch the game and go to your audio settings, then enable "Play sound while game is in background".


***


## 👾 32bit Drivers

#### Arch-based Distributions (Arch, EndeavourOS, Manjarno, etc)
1. Enable the [multilib repo](https://wiki.archlinux.org/title/Official_repositories#Enabling_multilib)
2. Install 32bit drivers
- AMD
  ```
  sudo pacman -S --needed lib32-mesa vulkan-radeon lib32-vulkan-radeon vulkan-icd-loader lib32-vulkan-icd-loader
  ```
- Nvidia
  ```
  sudo pacman -S --needed nvidia-dkms nvidia-utils lib32-nvidia-utils nvidia-settings vulkan-icd-loader lib32-vulkan-icd-loader
  ```

#### Ubuntu & Friends (Ubuntu, Mint, PopOS, etc)
- AMD
  ```
  sudo dpkg --add-architecture i386 && sudo apt update && sudo apt upgrade && sudo apt install libgl1-mesa-dri:i386 mesa-vulkan-drivers mesa-vulkan-drivers:i386
  ```
- Nvidia
  ```
  sudo dpkg --add-architecture i386 && sudo apt update && sudo apt install -y nvidia-driver-525 libvulkan1 libvulkan1:i386
  ```

#### Fedora & Derivatives (Fedora, Nobara, etc)
- TBA

#### openSUSE Tumbleweed
- AMD
  ```
  sudo zypper in kernel-firmware-amdgpu libdrm_amdgpu1 libdrm_amdgpu1-32bit libdrm_radeon1 libdrm_radeon1-32bit libvulkan_radeon libvulkan_radeon-32bit libvulkan1 libvulkan1-32bit
  ```
- Nvidia
   1. Follow the instructions here: https://en.opensuse.org/SDB:NVIDIA_drivers
   2. Install 32bit drivers by appending the `-32bit` suffix to the driver package names
   3. Install vulkan libraries:
      ```
      sudo zypper in libvulkan1 libvulkan1-32bit
      ```

#### Gentoo 💪
- We defer to your expertise
