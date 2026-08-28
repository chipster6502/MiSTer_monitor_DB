# MiSTer Monitor — Downloader Database

This is the [MiSTer Downloader](https://github.com/MiSTer-devel/Downloader_MiSTer)
custom database for the [MiSTer FPGA Monitor](https://github.com/chipster6502/MiSTer_monitor).

It installs and updates the **MiSTer-side server component** automatically
through the standard MiSTer Downloader workflow, without manually copying
files. A single script in the Scripts menu then handles all system
configuration for you.

> For the full installation guide — including the display firmware — see
> [`installation.md`](https://github.com/chipster6502/MiSTer_monitor/blob/main/docs/installation.md)
> in the main repository. This page only covers adding the database.

## What this database installs

| Path on MiSTer | Description |
|---|---|
| `Scripts/MiSTer_Monitor.sh` | Setup and launcher in one. Run from the Scripts menu it enables auto-start and `log_file_entry`, then restarts the server; it also accepts `start`, `stop`, `restart` and `status`. |
| `Scripts/start_monitor.sh` | Compatibility shim for installs made before the two scripts were merged. Nothing to run here. |
| `Scripts/MiSTer_Monitor_uninstall.sh` | Deactivates the monitor (stops the server, removes auto-start) while keeping the files in place. |
| `Scripts/.config/mister_monitor/mister_status_server.py` | The HTTP server that runs on the MiSTer and exposes core/game state to the display. |

## Installation

There are two ways to add this database to your MiSTer.

### Easiest: drop-in database

Download the drop-in database file:

[`downloader_chipster6502_MiSTer_monitor_DB.zip`](https://raw.githubusercontent.com/chipster6502/MiSTer_monitor_DB/db/downloader_chipster6502_MiSTer_monitor_DB.zip)

Extract the `.ini` file inside and place it next to `downloader.ini`
in the root of your MiSTer SD card (`/media/fat/`).

### Manual: edit downloader.ini

Add the following lines to the bottom of `/media/fat/downloader.ini`:

```ini
[chipster6502/MiSTer_monitor_DB]
db_url = https://raw.githubusercontent.com/chipster6502/MiSTer_monitor_DB/db/db.json.zip
```

## Installing and configuring

1. Run *Update All* or *Downloader* from your MiSTer Scripts menu. The files
   above are downloaded automatically.
2. Back in the Scripts menu, run **`MiSTer_Monitor`** once. It enables
   auto-start on boot, ensures `log_file_entry=1` in `MiSTer.ini`, and starts
   the server. It is safe to run again at any time.

That's it. For the display firmware (flashed to the device, not stored on the
MiSTer) and the rest of the setup, see the
[main installation guide](https://github.com/chipster6502/MiSTer_monitor/blob/main/docs/installation.md).

## Updates

Whenever the server-side files change in the main repository, this database is
updated to track them. The next time your MiSTer runs the Downloader, it picks
up the new versions automatically. You do **not** need to run
`MiSTer_Monitor` again after an update, although doing so restarts the server
so a freshly downloaded version takes effect without waiting for a reboot.

## Uninstallation

Run **`MiSTer_Monitor_uninstall`** from the Scripts menu. This *deactivates*
the monitor — it stops the server and removes the auto-start entry, but leaves
the files in place so you can re-enable it later by running
`MiSTer_Monitor` again (no re-download needed).

To remove it **completely**, after running the uninstall script also delete
the drop-in `downloader_chipster6502_MiSTer_monitor_DB.ini` from the root of
your SD card, so the Downloader stops tracking it.

> `log_file_entry=1` in `MiSTer.ini` is left untouched by the uninstall
> script, since other tools may rely on it. Set it back to `0` manually if
> you want.

## Related

- [Main repository (firmware, documentation, releases)](https://github.com/chipster6502/MiSTer_monitor)
- [MiSTer Downloader](https://github.com/MiSTer-devel/Downloader_MiSTer)
- [DB Template by theypsilon](https://github.com/theypsilon/DB-Template_MiSTer) (the template used to generate this database)

## License

MIT — same as the main repository.
