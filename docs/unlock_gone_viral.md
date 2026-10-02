# Unlock Gone Viral

This document shows the reverse engineering work done to understand how the Gone Viral trophy in Skate 3 for the PS3 is unlocked.

## Trophy Identification

### Background

PS3 games come with a `TROPHY.TRP` archive that describes every trophy the title
can award.
Parsing this archive gives us the game-defined identifier for Gone Viral that we can then search for in the game executable and save data.

### Trophy Details

The relevant entry from `TROPHY.TRP` is:

```
<trophy id="019" hidden="yes" ttype="B" pid="000">
  <name>Gone Viral</name>
  <detail>Catch the Skate Flu</detail>
</trophy>
```

| Field    | Value | Meaning |
|----------|-------|---------|
| `id`     | `019` | Decimal trophy index within this title |
| `hidden` | `yes` | Hidden in the XMB until earned; the description is not shown to the player beforehand |
| `ttype`  | `B`   | Bronze trophy |

### Key Findings

Trophy ID 19 (`0x13`) is the canonical identifier for Gone Viral within Skate 3.
This value is the handle the game engine uses when it calls the PS3 trophy service to award the trophy (`sceNpTrophyUnlockTrophy`).
Any save-file flag, in-game condition check, or code patch that targets this trophy will reference this ID.
