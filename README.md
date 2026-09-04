# bestrom_ota

OTA JSON catalog for BestROM.

- Branch `16` — Android 16 / BestROM A16 channel
- Branch `17` — Android 17 / BestROM A17 channel (VoltageOS 17 base)

The in-ROM updater fetches
`https://raw.githubusercontent.com/Mohithash/bestrom_ota/<branch>/<device>.json`,
set by a build-time resource overlay in
`vendor/bestrom/overlay/common/packages/apps/Updater/`. The changelog, if
present, is read from `changelog_<device>.txt` alongside it.

## Schema

`packages/apps/Updater/app/src/main/java/com/voltage/updater/misc/Utils.java:86-95`
reads each entry with `getLong`/`getString`, not the `opt` variants, so **all
six fields are mandatory** — a missing one throws and the whole feed fails to
parse:

```json
{
  "response": [
    {
      "timestamp": 1757000000,
      "filename": "BestROM-1.0-peridot-20260904-1042-UNOFFICIAL.zip",
      "md5": "<md5 of the zip>",
      "size": 1234567890,
      "download": "https://.../BestROM-1.0-peridot-....zip",
      "version": "1.0"
    }
  ]
}
```

`timestamp` is compared against the device's `ro.build.date.utc`
(`Utils.java:97-104`): an entry at or below it is discarded as "older than the
current build". So it must be the real build timestamp of the zip, and a
zero/placeholder entry is silently ignored rather than offered.

An empty `response` array is the correct state when no build has been
published — it parses cleanly and offers nothing.
