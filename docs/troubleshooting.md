# Troubleshooting

## Plugin does not load

- Confirm you are running a compatible **Paper** build, not standard Spigot.
- Check the exact Minecraft/Paper version required by the release.
- Review the server console for missing classes, unsupported API calls, or dependency errors.
- Confirm the downloaded JAR is complete and is the correct release.

## Menu does not open

- Run `/dm list` to check that the menu was loaded.
- Confirm the menu filename and menu ID.
- Check the menu's `permission` and any element/button `condition`.
- Review console output for YAML parse errors.
- Try `/dm open example` to separate a custom-menu problem from a general plugin problem.

## Changes do not appear

- Save the YAML file and run `/dm reload`.
- Confirm that you edited the active server's `plugins/sDialogMenus/menus/` directory.
- Check indentation, quotation marks, and filenames.

## Texture is missing

- Confirm the PNG exists under `plugins/sDialogMenus/textures/`.
- Check the exact filename and case.
- Run `/dm pack build`, then `/dm pack info`.
- Confirm the client has accepted and loaded the resource pack.
- Check the configured pack URL and firewall settings.
- Ensure the fallback text is configured so users still have a label if the texture is unavailable.

## Proxy counts or server switching do not work

- Confirm the server is behind a supported and correctly configured BungeeCord/Velocity proxy.
- Check that plugin messaging is enabled and the target server name is correct.
- Remember that proxy counts can show zero until a response arrives.

## Asking for support

When reporting an issue, include:
- sDialogMenus version;
- Minecraft client version and exact Paper build;
- relevant console error/stack trace;
- a minimal menu YAML that reproduces the issue;
- whether PlaceholderAPI or a proxy is installed.

Remove IP addresses, tokens, private URLs, and other sensitive details before posting logs or configuration.
