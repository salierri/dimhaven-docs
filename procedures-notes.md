# Procedures Notes

## Steam build checklist:

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


## PS porting

- Ticket a supportnak hogy az IP cím legyen felvéve amiről aktiválni lehet a konzolt (1-2 nap válaszidő)
- Content Pipeline-ban Concept, Product Group és Product elkészítése
  - Ehhez kell egy GDD, cserébe akármit elfogadnak
- DevNet-en létrehozni egy app-ot a CP ID-k alapján, kérni hogy legyen hozzá service (pár óra válaszidő)
- DevNet-en igényelni UDS és Trophy service-t is hozzá (pár óra válaszidő)
- GEMS-en fent átkattintani hogy melyik product, és letölteni az NP configot
- Konzolt beaktiválni VPN-es osztott netről (https://game.develop.playstation.net/resources/documents/SDK/10.000/DevKit-Setup_Guide/set-up-the-devkit.html)
- Leszedni egy SDK Manager-t innen: https://accounts.develop.playstation.net/sdk-manager/download/
- SDK managerbe belépni, letölteni egy vadiúj SDK-t (esetleg kidobni a régieket)
- PS Unity fórumról leszedni a Unity és SDK verziónk metszetéhez illő Unity plugint: https://p.siedev.net/forums/forum/1/
- PS Unity fórumról leszedni az InputSystem packaget (legújabbat), és behúzni package managerből custom tarballként
- Unitybe behúzni az NP configot (bemásolni és Player Settingsben referálni), legyártani a param.json-t és matchelni a kettőt

#### Üres projekt done, jöjjenek a hibák!

- FMOD accountot csinálni __ugyanazzal az emaillel mint a PS account__, igényelni projektet és összekötni a PS accal (1 nap válaszidő)
- FMOD-ról leszedni a PS FMOD plugint, behúzni unitybe
- Kódból `Keyboard.current` és `Mouse.current` referenciákat kipurgálni (legalábbis checkolni), ezek consoleon `null`-ok lesznek
- Figyelni rá hogy az NP config for whatever reason lepotyog és néha vissza kell húzni

- Shader errorok:
  - Volumetric fake lightokat kikapcsolni, azok DX specifikusak
  - ambiguous call to ‘lerp’ -> kódba belenézni, és a lerp-ekben figyelni rá hogy az első két paraméter típusa egyezzen
    - leggyakoribb eset hogy egy float mellett float3 van, fix: `0 -> float3(0, 0, 0)`
    - vagy ugyanez `fixed4`-el és barátaival. lehetséges fingerprintek: https://developer.download.nvidia.com/cg/lerp.html
    - ha nem ilyen egyszerű akkor pedig // és reménykedni hogy nem volt olyan fontos az a sor xd
  - Scriptable Render Pipelines packaget leszedni és behúzni custom tarballként innen: https://game.develop.playstation.net/forums/thread/19021/
  - E:\Unity\6000.0.26f1\Editor\Data\CGIncludes\UnityCG.cginc-be beleírni 695. sor

```
        return UnpackNormalmapRGorAG(packednormal); ->

        fixed3 normal;
        normal.xy = packednormal.xy * 2 - 1;
        normal.z = sqrt(1 - saturate(dot(normal.xy, normal.xy)));
        return normal;
        return UnpackNormalmapRGorAG(packednormal);
```

- Kód:
  - nyers fájlműveleteket kiszedni, filesystemhez csak mountolás után szabad nyúlni
  - input kezeléshez behúzni azt az 5-10 sort ami az InputSystemben van (TODO: rájönni hogy pontosan melyik kötelező belőle)


További TODO:
- EventSystem-re saját pointeres Input Module
- Víz visual bug (talán soften off megoldja)
- Save Game
- Egyéb TRC breaking dolgok átvarázsolása Quernből
- InputManager Awake/OnUnpairedDeviceUsed-jára ránézni hogy oda mi kötelező
- Controller fly hibákat generál

Amúgy kivettem a MainMenü-ből a start gamet, azt vissza lehet tenni

```
        if (InputManager.IsKeyPress("jump"))
            LoadScene.StartNewGame();
```

MenuEventHandler-ben ez ki volt //-ezve, talán nem véletlenül:

```
        if (QHelpers.MenuScene && MainMenuStartAnimation.IsInProgress())
            return;
```
