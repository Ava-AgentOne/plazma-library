# plazma-library

The filament bundle a **PLAzma** handheld downloads over Wi-Fi. Three files, and
nothing else lives here — this repo exists so the device can fetch them without
a credential.

| file | what it is |
|---|---|
| `manifest.json` | version, counts, and a SHA-256 for each file below |
| `library.idx` | one 64-byte record per colour — material, subtype, colour name, RGBA, weight, and the offset of its dump |
| `library.pack` | the dumps themselves, 1024 bytes each |

## How the device uses it

1. Fetch `manifest.json` and compare `version` with the copy on its SD card.
2. If it differs, download `library.idx` and `library.pack`.
3. **Verify both against the SHA-256 in the manifest**, and only then replace
   what is on the card. A half-written library means the device cannot identify
   filament at all, so a bad download must never reach the card.

## Where the data comes from

Built from **[queengooborg/Bambu-Lab-RFID-Library](https://github.com/queengooborg/Bambu-Lab-RFID-Library)**
— a community archive of genuine Bambu Lab tag dumps. All credit for the dumps
belongs there; this is only a repacking into two files a small device can seek
into without parsing a directory tree.

Regenerated with `shared/library/build_bundle.py` in the (private) PLAzma repo.
The subtype for each colour is **derived from the source library's own folder
names** rather than a hand-kept list, so two products can never collide into one
bucket.

## Why this repo is public when the project is not

It carries no PLAzma source — only a repacking of data that is already public.
Making it public is what lets the device fetch over plain HTTPS instead of
carrying a token in its flash, where anyone holding the device could read it.
