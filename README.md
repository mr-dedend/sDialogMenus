# sDialogMenus

> Build your own menus on the Minecraft **Dialog screen**, from YAML files — with or without textures.

**Author:** DedEnd · **Version:** 1.0.0 · **Platform:** Compatible Paper build with the Dialog API  
**Discord:** [discord.gg/rmSUTauBAM](https://discord.gg/rmSUTauBAM) · **Author Discord:** mr.dedend

---

## Features

- **Native menus** — text + buttons, no resource pack needed, works instantly
- **Texture menus** — custom PNG banners, canvas layouts, title logos — pack auto-generated
- **15 action types** — commands, console, sound, title, server switch, teleport, open other menus, delay chaining
- **14 placeholders** — player info, coordinates, online count, proxy server counts, PlaceholderAPI
- **Condition system** — `perm:`, comparisons, `&&` logic on any element/button
- **Camera backgrounds** — 3D scene behind the dialog, player frozen
- **Live overlay text** — real-time text drawn on top of texture banners
- **Auto pack build** — textures become font glyphs on every reload
- **Built-in HTTP server** — host the pack without external hosting
- **Per-menu commands** — any menu gets its own `/command` from YAML
- **open-on-join** — auto-open menus when players join
- **click-permission** — visible to all but clickable only with permission
- **Pack-extra folder** — merge custom fonts/sounds into the same pack

---

> **Documentation-only repository:** This repository contains user documentation only. It does not include plugin source code or the plugin JAR. Obtain the compiled JAR from the author's official download/resource page.

## Quick Start

1. Download the compiled `sDialogMenus.jar` from the author's official download/resource page.
2. Place `sDialogMenus.jar` into your server's `plugins/` directory
3. Start the server — example menus should be installed automatically
4. Run `/dm list` to see loaded menus
5. Run `/dm open example` to try the native menu
6. Run `/dm open games` to try the texture menu
7. Edit `plugins/sDialogMenus/menus/*.yml`, then run `/dm reload`

---

## Commands

| Command | Description | Permission |
|---|---|---|
| `/dm open <menu> [player]` | Open a menu (for others needs admin) | `sdialogmenus.open` / `sdialogmenus.admin` |
| `/dm close [player]` | Close dialog / leave camera | `sdialogmenus.admin` |
| `/dm list` | List all loaded menus | `sdialogmenus.admin` |
| `/dm reload` | Reload config, menus, rebuild pack | `sdialogmenus.admin` |
| `/dm pack build` | Force rebuild the resource pack | `sdialogmenus.admin` |
| `/dm pack info` | Show pack file, SHA-1, URL, textures | `sdialogmenus.admin` |
| `/dm pack send [player]` | Send pack to a player | `sdialogmenus.admin` |
| `/dm setcamera <name>` | Save current position as camera | `sdialogmenus.admin` |
| `/dm create <menu>` | Create new menu from template | `sdialogmenus.admin` |
| `/dm help` | Show help | everyone |

**Aliases:** `/dm` `/dmenu` `/sdm` `/sdmenu` `/sdialogmenus` `/dialogmenus`

**Permissions:**
- `sdialogmenus.open` (default: `true`) — open menus on yourself
- `sdialogmenus.admin` (default: `op`) — all admin commands

---

## File Structure

```
plugins/sDialogMenus/
├── config.yml                 # main configuration
├── menus/
│   ├── example.yml            # native menu example (no textures)
│   ├── games.yml              # texture banner example
│   └── layout.yml             # canvas free-layout example
├── textures/                  # your banner PNG files
│   ├── battle_royale.png
│   ├── duels.png
│   ├── zombie_survival.png
│   ├── logo.png
│   └── tpl_a.png ... tpl_f.png
├── cameras.yml                # saved camera positions (auto)
├── pack/
│   └── sDialogMenusPack.zip   # auto-generated resource pack
└── pack-extra/                # extra files merged into the pack as-is
```

---

## Menu File Format

### Top-Level Keys

```yaml
title: "<gold><bold>My Menu"     # MiniMessage title (fallback / no texture)
title-texture: logo.png           # PNG for the title bar (needs pack)
title-height: 48                  # displayed height in GUI px
title-ascent: 16                  # how far above baseline the logo sits
title-left-pad: 28                # offsets client warning button
title-slice: false                # slice the title texture

commands: [games, gameselect]     # commands that open this menu
open-on-join: false               # auto-open when player joins
join-delay-ticks: 20              # delay before auto-open (20 = 1s)
can-close-with-escape: true       # allow ESC (forced false if camera)
body-extra-width: 24              # extra px for text area (banners wrap if too small)
columns: 1                        # native buttons per row
permission: ""                    # who may open (empty = everyone)
no-permission-message: "<red>You do not have permission."
open-actions: []                  # actions run right after menu opens

background:                       # optional 3D camera scene
  enabled: false
  camera: arena
  restore-on-select: true
```

### Body Elements

Every element accepts `permission:` and `condition:` (hidden if not met).

#### Text
```yaml
- type: text
  text:
    - "<gray>Welcome, <white>%player%<gray>!"
    - "<gray>Online: <green>%online%"
  width: 300                     # 1–1024 px
```

#### Banner (texture, full width)
```yaml
- type: banner                   # aliases: image, texture
  texture: battle_royale.png     # file in plugins/sDialogMenus/textures/
  height: 54                     # GUI px — KEEP MULTIPLES OF 9
  ascent: 7                      # vertical alignment
  slice: false                   # true = force slicing (wide images auto-slice)
  hover: "<gray>Click to play"
  fallback: "<white>Battle Royale"  # shown when player has no pack
  click-permission: ""
  deny-message: "<red>No."
  overlays:                      # live text on top
    - text: "<green>%server_battleroyale% playing!"
      line: 1                    # 0 = top, each +1 = +9px
      align: right               # left | center | centre | right
      x: 6                       # offset from edge
      width: -1                  # force px (-1 = auto measure)
  actions:
    - "[server] battleroyale"
```

#### Canvas (free layout)
```yaml
- type: canvas                   # alias: layout
  width: 401                     # min width, grows if items reach further
  items:
    - texture: tpl_a.png
      x: 0                       # GUI px
      y: 0                       # MULTIPLE OF 9
      height: 36                 # MULTIPLE OF 9
      slice: true
      hover: "<gray>A"
      fallback: "<white>A"
      overlays: [...]
      actions:
        - "[message] <green>Clicked A"
```

#### Space
```yaml
- type: space                    # aliases: spacer, blank
  lines: 1
```

### Buttons

```yaml
buttons:
  - label: "<green>Spawn"        # MiniMessage
    tooltip: "<gray>Go to spawn"
    width: 140                   # 1–1024 px
    permission: ""
    condition: ""
    click-permission: ""
    deny-message: "<red>No."
    actions:
      - "[player] spawn"
```

### Exit Button

```yaml
exit-button:
  enabled: true
  label: "Close"
  tooltip: ""
  width: 150
  actions: []
```

Always added if: camera is used, or zero buttons exist.

---

## Actions (15 Types)

| Action | Syntax | Description |
|---|---|---|
| `[player]` | `[player] spawn` | Run command as player (alias: `[command]`) |
| `[console]` | `[console] give %player% diamond 1` | Run as server console |
| `[message]` | `[message] <yellow>Hi!` | Chat message (MiniMessage) |
| `[broadcast]` | `[broadcast] <gold>Event!` | Message to whole server |
| `[actionbar]` | `[actionbar] <green>Enjoy!` | Actionbar text |
| `[title]` | `[title] Title;Sub;10;60;10` | Title screen (fadeIn;stay;fadeOut ticks) |
| `[kick]` | `[kick] <gray>Bye.` | Kick player with reason |
| `[sound]` | `[sound] entity.experience_orb.pickup 1 1` | Play sound (volume pitch) |
| `[server]` | `[server] battleroyale` | Bungee/Velocity connect (alias: `[connect]`) |
| `[teleport]` | `[teleport] world 100 64 200 90 0` | Teleport (world x y z yaw pitch) |
| `[open]` | `[open] games` | Open another menu (alias: `[menu]`) |
| `[refresh]` | `[refresh]` | Re-open current menu |
| `[close]` | `[close]` | Close dialog + leave camera |
| `[delay]` | `[delay] 40` | Wait ticks then continue |
| `[menu]` | `[menu] games` | Alias for `[open]` |

**Notes:**
- Placeholders are resolved BEFORE the action runs
- No `[tag]` = defaults to `[player]`
- `[delay]` continues with the next action after the wait
- Quote all actions: `"[player] spawn"` (unquoted `[..]` is a YAML list)

**Example combining several:**
```yaml
actions:
  - "[sound] entity.experience_orb.pickup 1 1"
  - "[message] <green>Joining <white>Battle Royale</white>..."
  - "[delay] 10"
  - "[server] battleroyale"
```

---

## Placeholders

### Built-in

| Placeholder | Value |
|---|---|
| `%player%` / `%player_name%` | Player name |
| `%uuid%` | Player UUID |
| `%world%` | World name |
| `%x%` `%y%` `%z%` | Block coordinates |
| `%online%` | Online player count |
| `%max_players%` | Max players |
| `%server_<name>%` | Player count of proxy server `<name>` |

Works everywhere: titles, text, buttons, overlays, actions, hover, conditions.

### PlaceholderAPI

If installed, ALL PAPI placeholders work (e.g., `%vault_eco_balance%`, `%player_level%`). Auto-detected at runtime.

### Proxy Server Counts

`%server_<name>%` uses the BungeeCord plugin messaging channel. Requires a proxy (Bungee/Velocity) with the channel working. Counts refresh every `count-refresh-seconds` (default: 5s). Shows `0` until a count arrives.

---

## Conditions

Any element or button can have `condition:` — shown only if true. Empty = always true. Join with `&&`.

| Condition | Meaning |
|---|---|
| `perm:node` | Player has permission |
| `!perm:node` | Player does NOT have permission |
| `==` | Equals (case-insensitive for text, numeric for numbers) |
| `!=` | Not equals |
| `>=` `<=` `>` `<` | Numeric comparison |
| `contains` | Left contains right |
| `!contains` | Left does NOT contain right |
| `startswith` | Left starts with right |

**Examples:**
```yaml
condition: "perm:admin"
condition: "!perm:vip && %level% >= 10"
condition: "%player% == Notch"
condition: "%world% contains world"
condition: "perm:vip && %coins% >= 100"
```

**Note:** `||` (OR) is NOT supported. For OR behavior, duplicate the element with separate conditions.

---

## Resource Pack

### How It Works

1. Menu references a PNG from `textures/`
2. On `/dm reload` (or server start), the pack is auto-built
3. Each PNG becomes font glyphs (private-use codepoints) in a bitmap font
4. Players get the pack when they open a texture menu
5. Without the pack, banners fall back to `fallback` text

### Texture Rules

| Rule | Value |
|---|---|
| Design resolution | 2x (PNG is displayed at size/2) |
| Displayed height | MULTIPLE OF 9 (1 text line = 9px) |
| Good heights | 18, 27, 36, 45, 54, 72, 99... |
| Max height | 256px (hard MC limit — taller gets scaled down) |
| Width > 256px | Auto-sliced, full quality preserved |
| Transparency | Fully supported |

### Texture Mode

- `single` — one glyph when possible; wide images auto-sliced
- `sliced` — always cut into slices of `slice-width` px

### Pack Delivery (3 Methods)

**Method A — External URL:**
```yaml
resource-pack:
  url: "https://example.com/sDialogMenusPack.zip"
  sha1: ""          # from /dm pack info (optional but safer)
```

**Method B — Built-in HTTP server:**
```yaml
resource-pack:
  url: ""
  host:
    enabled: true
    port: 8123
    public-address: "your.public.ip.or.domain"
```
Open the port in firewall. Players get `http://address:8123/sDialogMenusPack.zip`.

**Method C — No pack:** Don't use textures. Native menus (text + buttons) work with zero configuration.

### Pack-Extra

Files in `plugins/sDialogMenus/pack-extra/` are merged into the zip unchanged. Use for custom fonts, sounds, extra textures.

---

## Camera Background

Give the dialog a live 3D background by teleporting the player to a saved spot.

**Setup:**
1. Stand where you want the view → `/dm setcamera arena`
2. In menu YAML:
   ```yaml
   background:
     enabled: true
     camera: arena
     restore-on-select: true
   ```
3. `/dm reload`

**Behavior:**
- Player frozen (can look around, cannot move, damage cancelled)
- `can-close-with-escape` forced `false` — must use exit button
- `restore-on-select: true` — leave camera on non-menu-opening actions
- `[open]`/`[refresh]` keep the camera
- `/dm close` or `[close]` action leaves camera

---

## config.yml

```yaml
count-refresh-seconds: 5       # proxy count refresh interval

resource-pack:
  enabled: true                 # false = texture menus fall back to text
  texture-mode: single          # single | sliced
  slice-width: 200              # slice size px (max 256)
  url: ""                       # external download link
  sha1: ""                      # pack SHA-1 (optional)
  host:
    enabled: false              # built-in HTTP server
    port: 8123
    public-address: ""
  force: false                  # true = kick players who decline
  prompt: "<gray>Loads the menu graphics"
  hide-warning-button: true     # hide client warning icon
  pack-format: 88               # MC resource pack format
  description: "sDialogMenus"
```

---

## Architecture

### Class Overview

| Class | Lines | Role |
|---|---|---|
| `sDialogMenus` | 772 | Main plugin: lifecycle, commands, pack sending, camera, proxy messaging |
| `MenuRenderer` | 371 | Turns `Menu` objects into Paper `Dialog` components |
| `PackBuilder` | 257 | Builds resource pack zip from PNG textures |
| `Menu` | 254 | Parses YAML menu files into typed objects |
| `Actions` | 132 | Executes action lists with delay chaining |
| `Placeholders` | 135 | Built-in placeholders, PAPI integration, condition evaluator |
| `Cfg` | 94 | Typed YAML reader wrapper |
| `PackHost` | 51 | Tiny HTTP server for pack hosting |
| `Spaces` | 38 | Invisible pixel-precise spacing via font space provider |
| `TextWidth` | 37 | Minecraft default font advance widths for overlay alignment |

**Total: ~2,035 lines of Java**

### Data Flow

```
YAML file → Menu.parse() → Menu object
                                ↓
                    MenuRenderer.build(player, menu, packOk)
                                ↓
                    Paper Dialog API → p.showDialog()
                                ↓
                    Click → Actions.run() → effects
```

### Pack Build Flow

```
All menu texture requests
    ↓
Load PNG from textures/ folder
    ↓
Scale down if >256px tall (hard limit)
    ↓
Slice if >256px wide or sliced mode
    ↓
Each slice → bitmap font glyph (private-use codepoints 0xE400–0xF8FF)
    ↓
Space font provider (0xE300–0xE32F for ±pixel spacing)
    ↓
pack.mcmeta + font JSON + glyph PNGs
    ↓
pack-extra/ files merged
    ↓
ZIP written → SHA-1 computed
```

### Key Design Decisions

1. **Texture as font glyphs** — Dialog API doesn't support images; text with custom font is the only way
2. **Space font provider** — invisible characters with exact advance widths for pixel-perfect overlay alignment
3. **Auto-slicing** — images wider than 256px are always sliced (MC glyph limit), never shrunk
4. **Height-only scaling** — images taller than 256px must scale down (hard MC limit)
5. **Consecutive banners share a body** — grouped into one text body so rows touch tightly
6. **Pack SHA-1 cache busting** — `?v=<sha1[0..8]>` in host URL forces client re-download
7. **Fixed zip timestamps** — stable SHA-1 across rebuilds (same content = same hash)
8. **Camera freeze via PlayerMoveEvent** — position locked, yaw/pitch free (look around)
9. **CommandMap reflection** — per-menu commands registered dynamically without plugin.yml changes
10. **Pending open queue** — menu waits for pack load before rendering (30s timeout → text fallback)

---

## Example Menus

### Native Menu (`example.yml`)
```yaml
title: "<gold><bold>Server Menu"
commands: [servermenu]
can-close-with-escape: true
columns: 2

body:
  - type: text
    text:
      - "<gray>Welcome, <white>%player%<gray>!"
      - "<gray>Players online: <green>%online%"
    width: 300
  - type: space
    lines: 1
  - type: text
    text: ["<red>You are an operator."]
    permission: "sdialogmenus.admin"

buttons:
  - label: "<green>Spawn"
    tooltip: "<gray>Go to spawn"
    width: 140
    actions:
      - "[sound] entity.experience_orb.pickup 1 1"
      - "[player] spawn"
  - label: "<aqua>Rules"
    tooltip: "<gray>Show the rules in chat"
    width: 140
    actions:
      - "[message] <yellow>1. Be kind  2. No cheating"
  - label: "<gold>Daily reward"
    tooltip: "<gray>Runs a console command"
    width: 140
    actions:
      - "[console] give %player% diamond 1"
      - "[title] <gold>Reward!;<gray>You got 1 diamond"
      - "[delay] 40"
      - "[actionbar] <green>Enjoy!"
  - label: "<red>Admin tools"
    tooltip: "<gray>Only for admins"
    width: 140
    permission: "sdialogmenus.admin"
    actions:
      - "[open] games"

exit-button:
  label: "Close"
  width: 150
```

### Texture Game Selector (`games.yml`)
```yaml
title-texture: logo.png
title-height: 48
title-ascent: 16
title-left-pad: 28
title: "<gold><bold>Hoplite"

commands: [games]
open-on-join: true
join-delay-ticks: 20
can-close-with-escape: false
body-extra-width: 24

background:
  enabled: false
  camera: arena
  restore-on-select: true

body:
  - type: banner
    texture: battle_royale.png
    height: 54
    hover: "<gray>Click to play"
    fallback: "<white><bold>Battle Royale</bold> <gray>- Last one standing wins"
    overlays:
      - text: "<green>%server_battleroyale% playing!"
        line: 1
        align: right
        x: 6
    actions:
      - "[sound] entity.experience_orb.pickup 1 1"
      - "[message] <green>Joining <white>Battle Royale</white>..."
      - "[server] battleroyale"
  # ... more banners ...

exit-button:
  enabled: true
  label: "Disconnect"
  width: 150
  actions:
    - "[kick] <gray>You left the server."
```

### Canvas Layout (`layout.yml`)
```yaml
title: "<gold><bold>Layout test"
commands: [layout]
can-close-with-escape: true
body-extra-width: 24

body:
  - type: canvas
    width: 401
    items:
      - texture: tpl_a.png
        x: 0
        y: 0
        height: 36
        slice: true
        hover: "<gray>Button A"
        fallback: "<white>A"
        actions: ["[message] <green>You clicked A"]
      # ... more items ...

exit-button:
  label: "Close"
  width: 150
```

---

## Troubleshooting

| Problem | Solution |
|---|---|
| Menu doesn't open | File must be `.yml` in `menus/`. `/dm reload` then `/dm list` |
| Banners show as text | Player has no pack. Set up `url` or `host` (see pack section) |
| Banners blurry/squeezed | Wide images auto-slice (no fix needed). Tall >256px scales down — keep height ≤256px |
| Banners wrap to two lines | Increase `body-extra-width` (e.g., 24 → 40) |
| Texture not found warning | PNG name must match file in `textures/` folder |
| `%server_x%` shows 0 | Needs proxy with BungeeCord channel. Check `count-refresh-seconds` |
| Command conflict | Console shows "already exists, skipped". Rename or remove clash |
| Camera doesn't restore | `restore-on-select` only triggers on non-menu-opening actions |
| Pack not downloading | Test URL in browser. Check firewall/port for host mode |
| Unknown action warning | Check action tag spelling in console log |

---

## Limits & Notes

- Dialog API is **experimental** — may change between Paper versions
- Dialog buttons can't hold images — textures use font glyphs in text body
- Background blur and warning icon come from client (icon made invisible by pack)
- Overlay text uses Minecraft default font metrics — for other fonts set `width:` manually
- Java 21+ required (compiled against Paper API 26.2.build.22-alpha)
- Resource pack `pack-format: 88` targets 1.21.x+ — update for newer clients

---

## License

Provided as-is. Contact DedEnd on Discord for support: [discord.gg/rmSUTauBAM](https://discord.gg/rmSUTauBAM)


---

## Documentation

- [Installation](docs/installation.md)
- [Configuration and menu files](docs/configuration.md)
- [Actions](docs/actions.md)
- [Placeholders and conditions](docs/placeholders.md)
- [Resource packs and textures](docs/resource-pack.md)
- [Troubleshooting](docs/troubleshooting.md)
- [Example configurations](docs/examples.md)

## Compatibility

sDialogMenus uses Paper's Dialog API and is **not compatible with a standard Spigot server in its current implementation**. A resource may be listed on SpigotMC, but the supported server software must be described accurately. Confirm the exact Paper version supported by the published JAR before release; the project metadata currently references an alpha Paper API dependency, so do not claim universal compatibility without testing.

## Support

For help, bug reports, or configuration questions, join the [Discord community](https://discord.gg/rmSUTauBAM). When reporting an issue, include the Minecraft client version, exact Paper build, plugin version, relevant configuration (remove private information), and console errors.

## Repository contents

This is a **documentation-only** repository. It intentionally does not contain Java source code, build scripts, or compiled server binaries.
