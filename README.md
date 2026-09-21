# PS4 Custom Avatar Pack Creator

Script and guide for building custom avatar packs for LAPY's Avatar Changer on a jailbroken PS4.

This guide covers how to unpack and repack the avatar fpkg, which was left as an open question in [F1nnish's original post](https://archive.org/download/ps4-jb-avatars). A small PowerShell script handles the whole process, so you don't have to edit any project files by hand.

---

## Requirements

- A jailbroken PS4 with **Avatar Changer by LAPY** installed
- **F1nnish's avatar pack and PS4 Xavatar Maker** (the converter): https://archive.org/download/ps4-jb-avatars
- **.NET Core 3.0 Runtime (x64)**, needed to run PkgTool: [download](https://dotnet.microsoft.com/en-us/download/dotnet/thank-you/runtime-3.0.3-windows-x64-installer)
- **PkgTool.Core** from [LibOrbisPkg](https://github.com/OpenOrbis/LibOrbisPkg) (included in this download)
- Windows with PowerShell (built into Windows 10 and 11)

---

## Folder Layout

Everything below goes in one folder, called "pkg maker" in this guide:

```
pkg maker/
├── converted/            <- your new .xavatar files go here
├── output/               <- your finished .pkg appears here
├── work/                 <- the extracted avatar pack (see One-Time Setup)
├── PkgTool.Core.exe
├── PkgTool.Core.dll
├── PkgTool.Core.runtimeconfig.json
├── LibOrbisPkg.dll
├── LibOrbisPkg.Core.dll
└── repack.ps1
```

---

## One-Time Setup

You only do this once. It extracts the original avatar pack into the "work" folder, which the script then reuses every time.

1. Download the avatar pack `.pkg` from F1nnish's archive.org page and put it in your "pkg maker" folder.
2. Open the "pkg maker" folder in File Explorer, click the address bar, type `cmd`, and press Enter.
3. Run this command (replace the file name with the pack you downloaded):

```
PkgTool.Core.exe pkg_makegp4 "Extra Avatars.pkg" work
```

If it asks for a passcode, add `--passcode 00000000000000000000000000000000` right after `pkg_makegp4`.

4. When it finishes, you should have a "work" folder containing `Project.gp4` and a `Media\StreamingAssets\AVATARS` folder full of `.xavatar` files.

Once the "work" folder exists, you can delete the original `.pkg` if you want the space back. The script doesn't need it.

---

## Step-by-Step Guide

1. Put avatars into the "png" folder of PS4 Xavatar Maker.
2. For "isonline.txt", keep "true" for offline activated account, and put "false" for offline account.
3. Run the exe "ps4-xplorer-avatar-maker".
4. It should then give you a folder called "converted".
5. Now copy said folder and put it into the pkg maker path.
6. Run the "repack.ps1".
7. Grab the PKG file (you can rename the PKG if you'd like) and put it onto your PS4.
8. After, run the app that it downloads, and it'll tell you what to do after.

To run the script (step 6), open the "pkg maker" folder in File Explorer, click the address bar, type `cmd`, press Enter, and run:

```
powershell -ExecutionPolicy Bypass -File .\repack.ps1
```

Your finished `.pkg` will be in the "output" folder.

---

## What the Script Does

1. Deletes the old avatars from `work\Media\StreamingAssets\AVATARS`.
2. Copies in every `.xavatar` file from your "converted" folder.
3. Updates the file list inside `Project.gp4` to match. The original is backed up as `Project.gp4.bak`.
4. Builds a new PKG into the "output" folder using PkgTool.

The script replaces **all** avatars in the pack with whatever is in "converted". If you add new avatars later and want to keep the old ones, leave the old `.xavatar` files in "converted" too.

---

## Troubleshooting

**PkgTool gives an error about .NET or a missing runtime.**
Install the .NET Core 3.0 Runtime (x64) from the link in Requirements. If you already have a newer .NET installed, you can also try adding a Windows environment variable named `DOTNET_ROLL_FORWARD` with the value `Major`.

**The avatars don't show up on the PS4.**
Check "isonline.txt" (step 2). It must be "true" for an offline activated account and "false" for an offline account. Re-run the converter and the script after changing it.

**The script says it can't find a file or folder.**
Make sure `repack.ps1`, `PkgTool.Core.exe`, and the "work" and "converted" folders are all in the same folder.

**The script says no .xavatar entries were found in Project.gp4.**
Make sure you extracted the pack with the One-Time Setup command, and that you're using one of F1nnish's avatar packs.

---

## Credits

- **F1nnish**: avatar packs and the Xavatar converter
- **LAPY**: Avatar Changer
- **OpenOrbis** and **maxton**: [LibOrbisPkg](https://github.com/OpenOrbis/LibOrbisPkg), used for unpacking and repacking the fpkg

PkgTool and LibOrbisPkg are licensed under the LGPL-3.0. The license file is included in this download.

---

## Disclaimer

This project is for personal homebrew use on a jailbroken PS4. Use it at your own risk. I'm not responsible for any issues with your console.
