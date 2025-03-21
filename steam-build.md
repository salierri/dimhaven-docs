# Steam build checklist

#### Preparations

- Bake Occlusion Culling
- Quick profiling to avoid performance bugs
- _Play_ and check no Errors and Warnings
- Check Project Settings/Player/Resolution and Presentation/Unity Splash Screen **OFF**
- Check Build settings for all scenes added (and no useless scenes added)
- Check **NO** Development Build

#### Build

- Target platform Windows, check Intel 64-bit -> Build
- Target platform Linux -> Build
- Target platform MacOS, **!! check Intel 64-bit + Apple silicon !!** -> Build
- Copy `steam_appid.txt` into Windows folder, next to `Dimhaven Enigmas.exe`
- Remove folder ending with `Do_Not_Ship` from Windows folder
- Copy `steam_appid.txt` into Linux folder, next to `Dimhaven Enigmas.x86_64`
- Remove folder ending with `Do_Not_Ship` from Linux folder
- Copy `steam_appid.txt` into Dimhaven Enigmas.app folder, **AND** copy it into the Contents folder as well (I'm still not sure which one is right lol)

#### Upload

- Zip everything one-by-one, on Windows and Linux, the contents of the folder, and on Mac, the .app folder
- Upload to steam via HTTP (https://partner.steamgames.com/apps/depotuploads/{appid})
- **Check build on Steam!**
