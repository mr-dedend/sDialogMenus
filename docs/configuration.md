# Configuration and Menu Files

Configuration is YAML. Indentation matters: use spaces, not tabs. Back up files before making changes.

## Folder structure

```text
plugins/sDialogMenus/
├── config.yml
├── menus/
│   ├── example.yml
│   ├── games.yml
│   └── layout.yml
├── textures/
├── cameras.yml
├── pack/
│   └── sDialogMenusPack.zip
└── pack-extra/
```

- `config.yml`: global plugin settings.
- `menus/*.yml`: one YAML file per menu.
- `textures/`: PNG images used by texture menus.
- `cameras.yml`: saved camera positions.
- `pack/`: generated resource pack.
- `pack-extra/`: additional resource-pack files merged into the generated pack.

## Top-level menu options

```yaml
title: "<gold><bold>My Menu"
title-texture: logo.png
title-height: 48
title-ascent: 16
title-left-pad: 28
title-slice: false

commands: [games, gameselect]
open-on-join: false
join-delay-ticks: 20
can-close-with-escape: true
body-extra-width: 24
columns: 1
permission: ""
no-permission-message: "<red>You do not have permission."
open-actions: []

background:
  enabled: false
  camera: arena
  restore-on-select: true
```

`title` uses MiniMessage formatting. `title-texture` requires a resource pack. Tick values use 20 ticks per second. A menu-level `permission` can restrict who may open a menu.

## Text element

```yaml
- type: text
  text:
    - "<gray>Welcome, <white>%player%<gray>!"
    - "<gray>Online: <green>%online%"
  width: 300
```

## Banner element

```yaml
- type: banner
  texture: battle_royale.png
  height: 54
  ascent: 7
  slice: false
  hover: "<gray>Click to play"
  fallback: "<white>Battle Royale"
  click-permission: ""
  deny-message: "<red>You do not have permission."
  overlays:
    - text: "<green>%online% players online"
      line: 1
      align: right
      x: 6
      width: -1
  actions:
    - "[message] <green>Selected!"
```

`banner` also accepts the aliases `image` and `texture`. Place the referenced PNG in `plugins/sDialogMenus/textures/`. For crisp results, check the example images and test dimensions in-game. Heights and canvas Y positions should generally use multiples of 9 for predictable line alignment.

## Canvas / layout element

```yaml
- type: canvas
  width: 401
  items:
    - texture: tpl_a.png
      x: 0
      y: 0
      height: 36
      slice: true
      hover: "<gray>Option A"
      fallback: "<white>Option A"
      overlays: []
      actions:
        - "[message] <green>Clicked A"
```

`canvas` also accepts `layout`. Items are positioned in GUI pixels. Avoid overlapping items; when items overlap, only the first item in the affected area may render as expected.

## Space element

```yaml
- type: space
  lines: 1
```

Aliases: `spacer` and `blank`.

## Buttons

```yaml
buttons:
  - label: "<green>Spawn"
    tooltip: "<gray>Go to spawn"
    width: 140
    permission: ""
    condition: ""
    click-permission: ""
    deny-message: "<red>You cannot use this button."
    actions:
      - "[player] spawn"
```

- `permission` / `condition` control whether the button is shown.
- `click-permission` can keep a button visible while restricting interaction.
- `actions` run when the button is selected.

## Exit button

```yaml
exit-button:
  enabled: true
  label: "Close"
  tooltip: ""
  width: 150
  actions: []
```

An exit button is automatically added when a camera is used or when a menu has no regular buttons.

## Per-menu commands and join menus

A menu can define `commands: [games, gameselect]` to register commands that open it. Use `open-on-join: true` to open it when a player joins and `join-delay-ticks` to delay opening.

## Element visibility

Elements and buttons can use `permission` and `condition`. See [Placeholders and Conditions](placeholders.md) for supported condition expressions.

## Reload changes

After editing menu YAML, run:

```text
/dm reload
```

Check the server console if a menu fails to load. YAML syntax errors and incorrect texture filenames are common causes.
