# Steam build checklist

#### Before big build

- Bake Occlusion Culling
- Quick profiling to avoid performance bugs
- _Play_ and check no Errors and Warnings

#### Generic Preparations

- Check `PROGRAM`, No Cheats and Dev Modes are enabled
- Check `PLAYER` position, should be standing on the landing platform (to avoid additional sound effects during start anim)
- Check Project Settings/Player/Resolution and Presentation/Unity Splash Screen **OFF**
- Check Build settings for all scenes added (and no useless scenes added)
- Check **NO** Development Build

##### Demo Preparations

- Check **both** `END_COLLIDER` gameobjects are in place (on the footpath)
- Check `PROGRAM`, Demo Build is enabled
- `landing_photo_challenge_leaflet_CINECAM` gameobject is **OFF**

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
