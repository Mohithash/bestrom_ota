# bestrom_ota

OTA catalog for the BestROM Updater.

- Branch `16` — Android 16 channel (retired)
- Branch `17` — Android 17 channel: `peridot.json` (POCO F6 / Redmi Turbo 3) and `changelog_peridot.txt`

The in-ROM Updater fetches `https://raw.githubusercontent.com/Mohithash/bestrom_ota/17/<device>.json`
and `changelog_<device>.txt`. Each entry in `response` needs `filename`, `download` (a public URL to the
full zip), `size` (bytes), `timestamp` (unix seconds) and `version`. The catalog is empty until a build is
hosted somewhere the phone can download 2.7 GB from; until then the Updater reports the device as up to date.

Official builds and their sha256 are listed in the changelog.
