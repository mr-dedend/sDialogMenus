# Installation

## Requirements

- A Paper server build compatible with the Dialog API used by your downloaded plugin JAR.
- A Minecraft client version compatible with that server.
- A resource pack is only required for menus that use custom textures.
- Optional: PlaceholderAPI for third-party placeholders.
- Optional: BungeeCord or Velocity proxy messaging configured for proxy server counts and server switching.

> **Compatibility:** The current implementation uses Paper's Dialog API and will not work on a standard Spigot server. Check the exact supported Paper version on the release page before installing.

## Install

1. Download `sDialogMenus.jar` from the author's official download/resource page.
2. Stop your server.
3. Place the JAR in the server's `plugins/` directory.
4. Start the server and check the console for errors.
5. Confirm that the `plugins/sDialogMenus/` folder has been created.
6. Run `/dm help` and `/dm list`.
7. Try `/dm open example`.

## Update

1. Back up `plugins/sDialogMenus/`, including your menus and textures.
2. Stop the server.
3. Replace the old JAR with the new release.
4. Start the server and review the console.
5. Test your custom menus and resource pack.

Do not delete your configuration or texture folders during an update unless the release notes explicitly require it.
