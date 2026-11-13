






# AppLock

AppLock is a small, local Windows application from **Cybersertex** that puts
selected desktop programs behind a master password. It is designed for Windows 10
and Windows 11 on both 32-bit (x86) and 64-bit (x64) systems.

Everything runs on your PC. AppLock does not use a network service, an account,
or a cloud backend.



<img width="2404" height="1586" alt="image" src="https://github.com/user-attachments/assets/f8bece9d-8dea-452f-a06a-5491c880a7a6" />

<BR>





## What it does

- **Protects applications you choose.** Pick from the applications installed on
  your PC, choose a running program, or browse to any `.exe`. While protection is
  on, AppLock watches for that program and closes it unless you approve it.
- **Sees what is installed.** The installed-app picker lists every app in your
  Start menu, including Microsoft Store apps, plus programs from the Windows
  uninstall registry, App Paths, and desktop shortcuts, so you do not have to hunt
  for executable files.
- **One master password.** The password is stored only as a salted PBKDF2-SHA256
  hash (210,000 iterations). It is never written in plain text. Hashing uses the
  Windows CNG implementation, so verifying a password stays fast. Password fields
  include a **Show password** eye button, and a wrong password shakes the dialog
  and plays the Windows error sound.
- **Fingerprint and Windows Hello.** On PCs where Windows Hello is set up, the
  unlock prompt offers a fingerprint **icon button**. It approves a protected
  application or opens the console with a fingerprint, face, or PIN, and the
  master password is always available as a fallback.
- **Recovery code and security questions.** During setup you receive a one-time
  recovery code and answer three security questions. Either method can reset a
  forgotten master password.
- **Encrypted settings.** Your configuration is stored in
  `%LOCALAPPDATA%\AppLock\settings.dat`, protected with Windows DPAPI for the
  current Windows user and marked hidden.
- **Temporary unlock.** Approve a protected application for a length of time you
  choose, or until you lock AppLock again.
- **External-device approval.** When enabled, a USB drive, SD card, or external
  disk that is connected while AppLock is running must be approved with the
  master password before it can be used. Drives that were already connected when
  AppLock started are treated as trusted.
- **Windows Safe Mode aware.** AppLock detects Safe Mode, shows clear Safe Mode
  indicators, and still requires the master password before it can be uninstalled.
- **Runs in the notification area.** Closing the window keeps AppLock running.
  Use the tray icon to reopen it.
- **Optional start with Windows.** AppLock can launch quietly when you sign in.
- **Self-uninstall.** On an installed copy, the uninstall button removes the
  program, its shortcut, the registry entry, and optionally the local settings.
- **Light, dark, or system appearance.** Choose a fully dark interface, a light
  interface, or follow the Windows app theme automatically. The setting applies
  immediately, including the native scrollbars.
- **WinUI 3 / Fluent interface.** The window uses a NavigationView-style sidebar,
  rounded cards, toggle switches, themed dropdowns, and a custom shield icon.
- **About section.** Settings shows the Cybersertex brand, version and
  architecture, data-folder location, developer details, and a contact link.

## Requirements

- Windows 10 or Windows 11 (32-bit or 64-bit).
- .NET Framework 4.8. It is included with current Windows 10 and Windows 11
  releases. If it is missing, install it from Microsoft.
- No administrator rights are required. AppLock installs per user.

## Install with Setup.exe

`AppLock-1.0.0-Setup.exe` is a single Cybersertex-branded installer that contains
both the 64-bit and 32-bit builds. It picks the correct one for the PC
automatically. The wizard uses the standard Windows setup window and native
controls, and shows which build it will install.

1. Run `AppLock-1.0.0-Setup.exe`.
2. Choose the install folder (the default is
   `%LOCALAPPDATA%\Programs\AppLock`).
3. Select whether to create a desktop shortcut, start AppLock when you sign in,
   and launch it after setup.
4. Choose **Install**, then **Finish**.

Setup creates Start menu and desktop shortcuts, registers the app in Windows
**Settings -> Apps**, and writes an uninstall entry under
`HKCU\Software\Microsoft\Windows\CurrentVersion\Uninstall\AppLock`. No
administrator prompt appears.

To remove AppLock later, use **Settings -> Apps -> AppLock -> Uninstall** or run:

```powershell
.\AppLock-1.0.0-Setup.exe --uninstall
```

### Silent setup

Setup accepts these command-line switches:

| Switch | Meaning |
| --- | --- |
| `/S` or `--silent` | Install without showing the wizard. |
| `/DIR=<path>` | Install to a specific folder. |
| `--startup` | Start AppLock when the user signs in. |
| `--no-desktop-shortcut` | Do not create a desktop shortcut. |
| `--no-launch` | Do not start AppLock after installing. |
| `--uninstall` | Run the uninstall wizard. |
| `--uninstall /S` | Uninstall silently. |
| `--remove-settings` | With uninstall, also delete AppLock settings. |
| `--password=<value>` | Master password for a silent uninstall in Safe Mode. |
| `--selftest` | Verify the embedded 64-bit and 32-bit payloads. |
| `--help` | Show the switch list. |

Example unattended install:

```powershell
.\AppLock-1.0.0-Setup.exe /S /DIR="$env:LOCALAPPDATA\Programs\AppLock" --startup
```

### Uninstalling in Safe Mode

Windows Safe Mode requires the AppLock master password before the app can be
removed. The uninstall wizard shows a password dialog. A silent uninstall must
include the password, or it stops without changing anything and exits with code
`2`:

```powershell
.\AppLock-1.0.0-Setup.exe --uninstall /S --password="your master password"
```

## Run without installing (portable)

1. Pick the ZIP that matches Windows:
   - `AppLock-1.0.0-win-x64.zip` for 64-bit Windows.
   - `AppLock-1.0.0-win-x86.zip` for 32-bit Windows.
2. Extract the ZIP to a folder such as `C:\Tools\AppLock`.
3. Run `AppLock.exe`.
4. Follow the four-step setup: create a master password, answer three security
   questions, save the recovery code, and finish.

The portable copy stores its settings in `%LOCALAPPDATA%\AppLock`. Removing the
folder removes the settings. To remove the program itself, delete the extracted
folder.

## Install from a ZIP with the PowerShell helper

The `install-applock.ps1` helper is an alternative to Setup.exe. It copies
AppLock into `%LOCALAPPDATA%\Programs\AppLock`, creates Start menu and desktop
shortcuts, and registers the same uninstall entry. Use it when you prefer a
script or are installing an extracted portable build.

Open PowerShell in the folder that contains `AppLock.exe` and run:

```powershell
powershell -NoProfile -ExecutionPolicy Bypass -File .\install-applock.ps1
```

Options:

| Option | Meaning |
| --- | --- |
| `-InstallDir <path>` | Install to a different folder. |
| `-StartWithWindows` | Also start AppLock when you sign in. |
| `-NoDesktopShortcut` | Skip the desktop shortcut. |
| `-NoLaunch` | Do not start AppLock after installing. |

After installing, use **Settings -> Uninstall AppLock** inside the program to
remove it. The uninstaller asks whether to delete the local settings.

## Using AppLock

The main window follows the Windows app theme by default (light or dark) and uses
a NavigationView-style sidebar:

- **Overview** shows the protection switch, a short status summary, statistics,
  and recent activity.
- **Applications** manages the protected list.
- **Settings** holds security, appearance, protection behavior, About, and
  maintenance options.

| Action | Where |
| --- | --- |
| Pause or resume protection | Overview page, protection switch |
| Lock now | Overview page or the sidebar |
| Choose an installed application | Applications -> Installed apps |
| Browse to an `.exe` file | Applications -> Browse .exe |
| Pick from running programs | Applications -> Running apps |
| Add by drag and drop | Drag an `.exe` onto the Applications window |
| Change unlock time | Settings -> Protection behavior |
| Require a password for new external drives | Settings -> Protection behavior |
| Change master password | Settings -> Change master password |
| Set or change security questions | Settings -> Security questions |
| Change light/dark appearance | Settings -> Appearance |
| Start with Windows | Settings -> Protection behavior |
| Open the data folder | Settings -> Open data folder |
| View developer and contact details | Settings -> About AppLock |
| Exit | Settings -> Exit AppLock |

When a protected program starts, AppLock closes it and shows a password prompt.
Enter the master password to allow the program for the configured unlock period.
Use the eye button in the password field to show or hide what you typed. A wrong
password shakes the prompt and plays the Windows error sound. If Windows Hello is
available, choose the fingerprint icon button to approve the program with a
fingerprint, face, or PIN instead. The master password still works, and it is the
only option when Windows Hello is not configured.

If you start a second copy of AppLock, it activates the copy that is already
running instead of starting another one.

### Adding installed applications

Choose **Applications -> Installed apps** to see the programs installed on this PC.
The list is built locally from five sources:

- the Start menu's own application list (`shell:AppsFolder`), which includes
  packaged Microsoft Store apps such as Calculator, Camera, Clock, Terminal,
  Photos, Spotify, and WhatsApp;
- programs registered in the Windows uninstall list (`HKLM` and `HKCU`, both
  32-bit and 64-bit views);
- executables registered under the `App Paths` registry keys;
- Start menu shortcuts (`.lnk` files), resolved to their target `.exe`;
- desktop shortcuts, including programs that are not registered anywhere else.

User-facing Store and packaged applications are included as well (Paint, Notepad,
Snipping Tool, Microsoft Store, Xbox, Teams, and similar). Background helpers,
uninstallers, installer copies, and AppLock itself are left out.

Use the search box to filter by name, publisher, or path. Click a row to toggle
its checkbox; a filled box with a check mark means the app is selected. Use
**Select all** to select every visible app and **Clear** to deselect them. Then
choose **Add selected**. Applications that are already protected appear with a
lock marker and cannot be selected again.

Packaged apps are launched through their Windows AppUserModelID when you approve
them, so they start exactly as they do from the Start menu.

If a program is missing from the list, choose **Browse for .exe...** in the same
dialog (or **Browse .exe** on the Applications page) to pick the executable
manually.

## External devices

Turn on **Settings -> Protection behavior -> Require password for external
drives** to ask for the master password before a newly connected drive can be
used. The option is off by default.

- AppLock watches for drives that appear after it starts: USB flash drives, SD
  cards, card readers, and external hard disks or SSDs, including newly
  appearing fixed volumes.
- When a drive arrives, AppLock shows a password prompt. Enter the master
  password to allow the drive for the configured temporary unlock period, or
  until you lock AppLock again.
- Choosing **Cancel** makes a best-effort attempt to remove the drive letter and
  dismount the volume. Windows can refuse when a file on the drive is open or
  when AppLock does not have administrator rights; in that case AppLock shows a
  warning and asks you to disconnect the drive manually.
- Drives that were already connected when AppLock started are trusted. Network
  drives, CD/DVD drives, the Windows system drive, AppLock's own drive, and the
  drive that holds the AppLock data folder are never prompted for or unmounted.

This is a convenience lock. It is not a kernel-level device firewall, and a user
who can open Disk Management or another disk tool can still access a drive.

## Windows Safe Mode

AppLock detects when Windows is running in Safe Mode and makes the state clear:

- a **SAFE MODE** badge in the title bar;
- a **WINDOWS SAFE MODE** label and status note on the Overview page;
- a Safe Mode note in **Settings -> About AppLock**;
- a Safe Mode entry in Recent activity and a startup notification.

In Safe Mode, uninstalling AppLock still requires the master password. The
in-program uninstall flow asks for it, and Setup.exe shows a Safe Mode password
dialog. A silent Setup.exe uninstall needs `--password=<value>` and exits with
code `2` when the password is missing or incorrect.

> **Startup limitation:** Windows does not run `HKCU` Run-key startup entries in
> Safe Mode. If you need AppLock in Safe Mode, start `AppLock.exe` manually after
> signing in. The **Start AppLock when I sign in** setting applies to normal
> startup only.

Safe Mode is a diagnostic environment. AppLock's protections remain
convenience-level there, and an administrator can still bypass them.

## Recovery

If you forget the master password:

1. Open AppLock and choose **Forgot password?** on the unlock prompt.
2. Use either recovery path:
   - enter the recovery code you saved during setup; or
   - choose **Use security questions instead** and answer the three questions.
3. Choose a new master password.

Security answers are not case-sensitive, and repeated spaces are ignored. You can
change the questions' answers at any time in **Settings -> Security questions**.

The recovery code is shown only once. AppLock cannot recover it later. If both
the password and the recovery options are lost, delete
`%LOCALAPPDATA%\AppLock\settings.dat` to start over; this also removes the list of
protected applications.

## Fingerprint and Windows Hello

AppLock can use **Windows Hello** to approve a protected application or unlock the
console. It works with a fingerprint reader, an infrared camera for face unlock,
or the Windows Hello PIN, depending on what the PC supports and what is configured
in **Windows Settings -> Accounts -> Sign-in options**.

- When the unlock prompt opens, AppLock checks whether Windows Hello is available.
  If it is, a fingerprint **icon button** and a short hint appear.
- Choose the button and complete the Windows Hello prompt. A successful check
  approves the action without typing the master password.
- Canceling the Windows Hello prompt leaves the password field ready, so you can
  always fall back to the master password.
- If Windows Hello is not configured, is disabled by policy, or the PC has no
  compatible device, the button is hidden and the password prompt works as before.
- AppLock never receives or stores fingerprint, face, or PIN data. Verification is
  performed entirely by Windows, and the master password lockout rules still apply
  when the password field is used.

## Appearance

**Settings -> Appearance** offers three modes:

| Mode | Behavior |
| --- | --- |
| **System** | Follows the Windows app theme and updates when Windows changes. |
| **Light** | Always uses the light palette. |
| **Dark** | Always uses the fully dark palette. |

The choice is saved with your other settings and applies immediately. AppLock
also applies the native dark scrollbar theme to its scrolling pages.

## About Cybersertex

AppLock is developed and published by **Cybersertex**.

- Developer: Sumit Ghosh
- Email: [cybersertex@outlook.com](mailto:cybersertex@outlook.com)

The same details appear in **Settings -> About AppLock**, together with the app
version, architecture, install state, and data-folder path.

## Command line

| Argument | Effect |
| --- | --- |
| `--minimized` | Start in the notification area. |
| `--uninstall` | Begin the uninstall flow. Used by the registered uninstaller. |
| `--selftest` | Run internal checks and write a log to `%TEMP%\AppLock-selftest.log`. |
| `--register` | Register the uninstall entry for the current folder. |
| `--list-installed` | Print every application the installed-app scanner finds, then exit. |

Developer test hooks (not needed for normal use):

| Environment variable | Effect |
| --- | --- |
| `APPLOCK_SIMULATE_DEVICE=<drive root>` | Pretend a drive arrived after startup, without plugging in hardware. |
| `APPLOCK_SAFE_MODE=1` | Force AppLock and Setup to behave as if Windows is in Safe Mode. |

## Security notes and limits

- AppLock is an application-level convenience lock. It is not a full security
  boundary. A Windows administrator or another account can bypass it. Safe Mode
  still requires the master password to uninstall, but Safe Mode itself is a
  diagnostic environment where an administrator can change the system.
- A user who can edit AppLock's files or kill its process can bypass it. Do not
  use it as the only protection for sensitive data.
- External-device protection covers drives connected while AppLock is running.
  Drives already present at startup are trusted, and unmounting is best-effort:
  Windows can refuse when the drive is in use or AppLock is not elevated.
- Windows does not run `HKCU` Run-key startup entries in Safe Mode. Start
  AppLock manually if you need it while Windows is in Safe Mode.
- Protected programs are matched by executable file name. A renamed copy of a
  program is treated as a different program.
- AppLock does not encrypt your files or your Windows account.
- Windows Hello verification is performed by Windows. AppLock never receives or
  stores biometric data, and the security of that check depends on how Windows
  Hello is configured for the Windows account.
- Settings are tied to the Windows user who created them through DPAPI. Copying
  `settings.dat` to another user or PC will not decrypt.

## Building from source

The project is plain C# for .NET Framework 4.8 and does not need a .NET SDK or
Visual Studio. It builds with the C# compiler that ships with Windows.

Requirements:

- Windows 10 or Windows 11.
- .NET Framework 4.8 runtime.

Build both platforms:

```powershell
powershell -NoProfile -ExecutionPolicy Bypass -File .\build.ps1
```

Build one platform:

```powershell
powershell -NoProfile -ExecutionPolicy Bypass -File .\build.ps1 -Platform x64
powershell -NoProfile -ExecutionPolicy Bypass -File .\build.ps1 -Platform x86
```

`Setup.exe` is created only by a full build (or when both `dist\win-x64` and
`dist\win-x86` already exist), because it embeds both payloads.

The convenience wrappers `build-all.bat`, `build-x64.bat`, and `build-x86.bat`
call the same script.

Output:

```
dist\
  win-x64\                 AppLock.exe and support files (64-bit)
  win-x86\                 AppLock.exe and support files (32-bit)
  AppLock-1.0.0-win-x64.zip
  AppLock-1.0.0-win-x86.zip
  AppLock-1.0.0-Setup.exe   Combined installer (both architectures)
  SHA256SUMS.txt
```

The build runs `AppLock.exe --selftest` for each platform and
`AppLock-1.0.0-Setup.exe --selftest` for the installer, and fails if any check
does not pass. The build writes the SHA256 for every artifact to
`dist\SHA256SUMS.txt`. You can verify a downloaded file with:

```powershell
Get-FileHash .\AppLock-1.0.0-Setup.exe -Algorithm SHA256
```

## Project layout

```
AppLock\
  build.ps1                     Build both platforms, ZIPs, and Setup.exe
  build-all.bat                 Wrapper for build.ps1
  build-x64.bat                 Wrapper for build.ps1 -Platform x64
  build-x86.bat                 Wrapper for build.ps1 -Platform x86
  build-test\                   Automated UI, guard, unlock, device, Safe Mode, recovery, and setup tests
  README.md
  LICENSE
  assets\                       Application shield icon and preview
  src\AppLock\                  Application source
  src\AppLockSetup\             Setup.exe source (embeds both app payloads)
  tools\install-applock.ps1     Per-user installer script
  tools\make-icon.ps1           Regenerates the icon
```

## License

MIT. See `LICENSE`.
