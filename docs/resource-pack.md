# Resource Packs, Textures, and Overlays

Native text-and-button menus can work without a resource pack. Custom PNG banners, title textures, and canvas graphics require the generated resource pack.

## How it works

sDialogMenus builds a resource pack from PNG images and custom bitmap-font glyphs so the images can be rendered in the Dialog interface. The generated archive is stored at:

```text
plugins/sDialogMenus/pack/sDialogMenusPack.zip
```

## Add a texture

1. Stop or keep the server running while editing only if your normal workflow permits it; back up first.
2. Copy a PNG into `plugins/sDialogMenus/textures/`.
3. Reference the exact filename in your menu YAML.
4. Run `/dm pack build` or `/dm reload`.
5. Configure pack delivery and test with a client that accepts the pack.

Example:

```yaml
- type: banner
  texture: my_banner.png
  height: 54
  ascent: 7
  slice: false
  fallback: "<gold>My Banner"
  actions:
    - "[open] example"
```

## Resource-pack delivery options

Configure the relevant options in `plugins/sDialogMenus/config.yml`:

1. **External URL:** Host the generated ZIP and set the resource-pack URL.
2. **Built-in HTTP hosting:** Enable the plugin's host option, set its port and public address, and ensure the port is reachable from the internet.
3. **No custom graphics:** Use native text and buttons only; resource-pack delivery is not needed for those menus.

Use `/dm pack info` to inspect the generated pack details and SHA-1, and `/dm pack send [player]` to send the pack where supported.

## Pack-extra

Files in `plugins/sDialogMenus/pack-extra/` are merged into the generated pack. Keep the expected Minecraft resource-pack directory structure, for example `assets/<namespace>/...`.

## Overlay text

Banners can render dynamic text overlays:

```yaml
overlays:
  - text: "<green>%online% online"
    line: 1
    align: right
    x: 6
    width: -1
```

- `line`: vertical line index; `0` is the first line.
- `align`: `left`, `center`/`centre`, or `right`.
- `x`: horizontal edge offset.
- `width: -1`: let the plugin measure text width automatically.

## Common pack issues

- Confirm the texture filename and capitalization match.
- Rebuild the pack after changing images.
- Confirm the configured URL is publicly reachable from the Minecraft client.
- Check firewall/NAT rules if using built-in hosting.
- Check the server/client version's resource-pack format compatibility.
- If a player declines or fails to load the pack, texture elements may show fallback text instead.
