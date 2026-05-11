# MiSTer Monitor — Downloader Database

This is the [MiSTer Downloader](https://github.com/MiSTer-devel/Downloader_MiSTer)
custom database for the [MiSTer FPGA Monitor](https://github.com/chipster6502/MiSTer_monitor).

It lets users install and update the **MiSTer-side server component**
(`mister_status_server.py` and `start_monitor.sh`) automatically through
the standard MiSTer Downloader workflow, without manually copying files
or editing scripts.

## What this database installs

| Path on MiSTer | Description |
|---|---|
| `Scripts/start_monitor.sh` | Launcher script visible in the MiSTer Scripts menu (`start`, `stop`, `restart`, `status`). |
| `Scripts/.config/mister_monitor/mister_status_server.py` | The HTTP server that runs on the MiSTer and exposes core/game state to the second screen. |

## Installation

There are two ways to add this database to your MiSTer.

### Easiest: drop-in database

Download the drop-in database file:

[`downloader_chipster6502_MiSTer_monitor_DB.zip`](https://raw.githubusercontent.com/chipster6502/MiSTer_monitor_DB/db/downloader_chipster6502_MiSTer_monitor_DB.zip)

Extract the `.ini` file inside and place it next to `downloader.ini`
in the root of your MiSTer SD card (`/media/fat/`). That's it — the
next time you run *Update All* or *Downloader* from your MiSTer, the
files will be downloaded automatically.

### Manual: edit downloader.ini

Add the following lines to the bottom of `/media/fat/downloader.ini`:

```ini
[chipster6502/MiSTer_monitor_DB]
db_url = https://raw.githubusercontent.com/chipster6502/MiSTer_monitor_DB/db/db.json.zip
```

Then run *Update All* or *Downloader* as usual.

## After installation

Once the files are downloaded, you still need to do a few one-time
configuration steps on the MiSTer:

1. Make `start_monitor.sh` executable (the Downloader preserves
   permissions, but if it doesn't work, run `chmod +x /media/fat/Scripts/start_monitor.sh`).
2. Add this line to `/media/fat/linux/user-startup.sh` so the server
   starts automatically on boot:
```bash
   /media/fat/Scripts/start_monitor.sh start
```
3. Enable `log_file_entry=1` in `/media/fat/MiSTer.ini` (under the
   `[MiSTer]` section). This is required for the server to detect
   core and game changes.
4. Reboot the MiSTer or start the server manually:
```bash
   /media/fat/Scripts/start_monitor.sh start
```

For the **Tab5 firmware** (which is flashed to the device, not stored on
the MiSTer), follow the instructions in the
[main repository README](https://github.com/chipster6502/MiSTer_monitor#tab5-side).

## Updates

Whenever a new version of the server-side scripts is released in the
main repository, this database is updated to track it. The next time
your MiSTer runs the Downloader, it picks up the new files
automatically.

## Uninstallation

Remove the line you added to `downloader.ini`, then run *Update All* or
*Downloader* again — the Downloader detects the removed entry and
cleans up the files it had installed from this database.

## Related

- [Main repository (firmware, documentation, releases)](https://github.com/chipster6502/MiSTer_monitor)
- [MiSTer Downloader](https://github.com/MiSTer-devel/Downloader_MiSTer)
- [DB Template by theypsilon](https://github.com/theypsilon/DB-Template_MiSTer) (the template used to generate this database)

## License

MIT — same as the main repository.