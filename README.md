# Chocofi right-half: Handy remote + Helix + Kindle

Repurposes the **right half only** of a [Chocofi](https://github.com/pashutk/chocofi) (36-key, nice!nano) as a standalone BLE remote. Three hosts, no display.

BLE name: **`Chocofi-Handy`**.

Shield files are vendored from `willnapier/temper-zmk`. `CONFIG_ZMK_SPLIT=n` so the right controller is a normal keyboard, not a split peripheral.

## Hosts

Combos are adjacent home-row pairs, **from rest only** (`require-prior-idle-ms` 1000, `timeout-ms` 300). Mash both keys together; a roll of `e` then `i` will type letters instead. Same combo always selects that host — not a toggle. Combos change the BLE slot only; the letter base stays.

| Combo | Profile | Machine |
|-------|---------|---------|
| **n+e** | 0 | iPhone |
| **e+i** | 1 | nimbini |
| **i+o** | 2 | Mac |

A Mac is one bond. Kindle-on-Mac is the PAGE layer (outer thumb hold), not a fourth profile.

After reboot the bond returns; the layer is the letter base. Pair nimbini first (`e+i`) so slot 1 is the usual computer.

## Keys (Temper right-half letters)

```
    J    L    U    Y    '
    M    N    E    I    O
    K    H    ,    .    p      ← this key was / on Temper
 inner  mid  outer
 Enter  Bspc Space
```

No home-row mods.

### Thumbs

| Thumb | Tap | Hold |
|-------|-----|------|
| **Inner** (Temper Enter) | Enter | HELIX (hold-preferred; hold then tap `n` for Handy) |
| **Middle** (Temper Backspace) | Backspace | Shift |
| **Outer** (Temper Shift; **Space on this device**) | Space | PAGE |

### Combos (BASE)

| Keys | Sends |
|------|-------|
| Y+' | Esc |
| J+L | `:` |
| I+. | `:` |
| M+N | `-` |
| N+H | `-` |
| inner + `n` | F20 Handy toggle |
| inner + `e` | F19 Handy cancel |
| inner + `m` | `t` insert |

### Inner thumb held (HELIX)

| Key (unshifted) | Finger | Sends |
|-----------------|--------|-------|
| `m` | index, 1u left of home | `t` (Helix insert) |
| `n` | index home | F20 (Handy toggle) |
| `e` | middle finger | F19 (Handy cancel) |
| `i` | ring | unbound (must not send Ctrl/Cmd+Z) |
| `o` | pinky | `BT_CLR_ALL` (wipe bonds, start advertising) |
| `k` | index, 1u left, bottom | `x` |
| `p` (old `/`) | pinky, bottom | `/` |

Undo-paste is base **`u`**.

**Bootloader:** Y+' **and** inner thumb, from rest (~1 s idle). Hardware recovery: nice!nano double-tap reset, or RST–GND.

**Sleep / wake:** deep sleep after 15 minutes. The matrix is a `wakeup-source`, so any key (a thumb is easiest) should wake it — do not use the nice!nano reset button for that. Short reset still works if a key does not. Inner-hold + Y+' is bootloader only, and only while already awake.

### Outer thumb held (PAGE)

| Key | Sends |
|-----|-------|
| `n` | Left |
| `o` | Right |
| `e` | Down |
| `i` | Up |

## Flash

If this nice!nano already has other firmware, **`settings_reset.uf2` first**, then the Handy UF2, then pair:

1. Double-tap reset on the **right** nice!nano → `NICENANO` drive.
2. Flash **`settings_reset.uf2`** once.
3. Double-tap reset again.
4. Flash **`chocofi_right_handy.uf2`**.
5. Combo **e+i**, pair **Chocofi-Handy** on nimbini.
6. Combo **i+o**, pair on the Mac.
7. Combo **n+e**, pair on the iPhone.

Use `cp -X` on macOS if Finder copies grow xattrs and the board rejects the UF2.

## Build

GitHub Actions (push to `main`, or *Run workflow*) uploads a `firmware` artifact (`chocofi_right_handy.uf2`, `settings_reset.uf2`).

Host binds (F20 toggle, F19 cancel) and the Handy loop: `~/Assistants/shared/LOCAL-DICTATION.md`.
