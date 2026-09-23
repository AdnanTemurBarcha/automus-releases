<div align="center">

# Automus

**Every dev server. One window.**

A desktop launcher for local development servers. Register each project's folder and start command once, then run, stop and watch all of them from a single window.

[**Download the latest release**](https://github.com/AdnanTemurBarcha/automus-releases/releases/latest) ·
[Website](https://automus.nexylius.com) ·
[Guide](https://automus.nexylius.com/run-multiple-dev-servers/) ·
[Privacy](https://automus.nexylius.com/privacy/)

macOS (Apple Silicon and Intel) · Windows · Linux

</div>

---

## What this repository is

This is the **public releases repository for Automus**. It holds the installers and nothing else: every release here is built automatically from the Automus source repository and published to this page so anyone can download it without an account.

The Automus application source is private. There is no code to browse here. To get the app, use the [Releases](https://github.com/AdnanTemurBarcha/automus-releases/releases) page; to learn what it does, read below or visit [automus.nexylius.com](https://automus.nexylius.com).

## What is Automus?

Working across several projects usually means a different start command for each — `php artisan serve` here, `npm run dev` there, a GeoServer `startup.sh` somewhere else — each in its own terminal tab and its own folder, retyped after every login. Stopping them is worse: closing a shell does not always stop the server it started, and the port stays taken.

Automus saves each project once and turns it into a button.

- **Register a project** — pick its root folder and the command that starts it. It is remembered between launches.
- **Run it** — press ▶. A project's default command runs in a fixed slot, so pressing it again *restarts that same run* instead of leaving a duplicate process behind.
- **Run anything else beside it** — press **+** to start any other command in that project's folder (a queue worker, a watcher, a migration) as an independent, concurrent run.
- **Watch every run live** — each run gets its own tab with color-coded output: URLs, HTTP methods, status codes, warnings and errors are highlighted.
- **Run All / Stop All** — bring a whole stack up after login and down again before you close the lid.
- **Manage from the sidebar** — right-click a project to edit it, stop all of its runs, or delete it.

It works with anything you would start from a shell, for example:

```
php artisan serve
npm run dev
./startup.sh
python manage.py runserver
docker compose up
```

Automus is a launcher for servers that run on your machine. It is not a container orchestrator and not a terminal multiplexer — if your whole stack already lives in `docker compose`, keep using that.

## Downloads

Each release contains four files. Pick the one for your system:

| System | File | Notes |
| --- | --- | --- |
| **macOS — Apple Silicon** (M1, M2, M3 and later) | `Automus-<version>-macOS-arm64.dmg` | Drag to Applications |
| **macOS — Intel** | `Automus-<version>-macOS-x86_64.dmg` | Drag to Applications |
| **Windows** | `Automus-Setup-<version>.exe` | Installer with Start Menu shortcuts and an uninstaller |
| **Linux** (x86_64) | `Automus-<version>-x86_64.AppImage` | Single file, no install step |

**Not sure which Mac you have?** Open the Apple menu → **About This Mac**. If the chip says *Apple M…*, use the Apple Silicon build. If it says *Intel*, use the Intel build.

[**→ Go to the latest release**](https://github.com/AdnanTemurBarcha/automus-releases/releases/latest)

## Installing

> **Builds are not code-signed yet.** Your operating system will warn about an "unidentified developer" the first time you open Automus. That is expected, and the steps below get past it once.

### macOS

1. Open the `.dmg` and drag **Automus** onto the **Applications** shortcut.
2. Open **Applications**, then **right-click Automus → Open**, and confirm.
3. On **macOS 15 (Sequoia) or later** the right-click shortcut may not offer *Open*. In that case, try to open the app once, then go to **System Settings → Privacy & Security**, scroll to the message about Automus and click **Open Anyway**.

You only need to do this the first time.

### Windows

1. Run `Automus-Setup-<version>.exe` and follow the wizard.
2. If SmartScreen says *"Windows protected your PC"*, click **More info**, then **Run anyway**.

Automus installs to Program Files, adds Start Menu and desktop shortcuts, and can be removed from **Settings → Apps** like any other program.

### Linux

1. Make the file executable, then run it:

   ```bash
   chmod +x Automus-*-x86_64.AppImage
   ./Automus-*-x86_64.AppImage
   ```

2. If it does not start, your system may be missing FUSE 2, which AppImages use. On Ubuntu and Debian:

   ```bash
   sudo apt install libfuse2
   ```

   or run it without FUSE:

   ```bash
   ./Automus-*-x86_64.AppImage --appimage-extract-and-run
   ```

The bundle includes the Qt libraries. A few system libraries (xcb, fontconfig) are assumed to be present, as they are on virtually every desktop distribution.

## Using Automus

1. Click **New Project**, choose the project's root folder and enter the command that starts it.
2. Click **▶** on the project's row to run it, or **Run All** in the toolbar.
3. Use **+** on a row to run a one-off command in that folder alongside the default one.
4. Each run has its own tab. Use **Stop** in the tab's toolbar (or the ■ on its sidebar row) to stop that run, and **Clear log** to empty its output.

A run's status is shown by the dot beside it: **green** running, **grey** idle, **red** error.

### How it behaves

- **Your normal PATH.** On macOS and Linux, every command runs through your own login shell, so tools added by Homebrew, nvm, XAMPP and similar resolve exactly as they do in your terminal — even though the app was opened from the Dock or a launcher.
- **Stop reaches the whole tree.** On macOS and Linux each run starts in its own process group. Stop sends `SIGTERM` to the entire group, then `SIGKILL` after a three-second grace period, so child processes started by your command do not survive as orphans holding a port.
- **One default run per project.** Pressing ▶ again restarts the same run; it never starts a second copy of the default command.

## Your data and privacy

- **Local only.** Automus has no account, no sign-in, no analytics and no telemetry, and it makes **no network requests of its own**.
- **Where your projects are stored** — a single JSON file in your operating system's application-data folder. Typically:

  | System | Location |
  | --- | --- |
  | macOS | `~/Library/Application Support/Automus/config.json` |
  | Windows | `%APPDATA%\Automus\config.json` |
  | Linux | `~/.local/share/Automus/config.json` |

- **Commands run as you.** Automus starts the commands you give it, in the folders you choose, with your own user account's permissions. Any network traffic they generate is theirs, not Automus's. Their output is shown in the app and is not saved or sent anywhere by Automus.

Full policy: [automus.nexylius.com/privacy](https://automus.nexylius.com/privacy/).

## Uninstalling

- **macOS** — drag Automus from Applications to the Trash.
- **Windows** — uninstall it from **Settings → Apps**.
- **Linux** — delete the AppImage.

To also remove your saved projects, delete the `config.json` file listed above (or the `Automus` folder that contains it).

## Updating

Automus does not update itself. To update, download the newest release from the [Releases](https://github.com/AdnanTemurBarcha/automus-releases/releases) page and install it over the old one — your saved projects are kept, because they live outside the app.

## Reporting a problem or asking for a feature

Please [open an issue](https://github.com/AdnanTemurBarcha/automus-releases/issues) in this repository. It helps to include:

- your operating system and version, and whether it is Apple Silicon or Intel on a Mac,
- the Automus version (the release tag you downloaded),
- what you did, what you expected, and what happened instead.

## About

Automus is built by [Adnan Temur Barcha](https://adnantemurbarcha.nexylius.com) and published under the [Nexylius](https://nexylius.com) brand, alongside other local-first tools.

The application source is private; only the installers are published here.
