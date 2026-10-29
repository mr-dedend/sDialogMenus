# Actions

Actions are configured as quoted YAML strings. Quote them because unquoted square brackets can be interpreted as YAML syntax.

## Supported actions

| Action | Example | Purpose |
|---|---|---|
| `player` | `"[player] spawn"` | Run a command as the player; `[command]` is an alias. |
| `console` | `"[console] give %player% diamond 1"` | Run a command as console. |
| `message` | `"[message] <yellow>Hello!"` | Send a MiniMessage-formatted message to the player. |
| `broadcast` | `"[broadcast] <gold>Event starts now!"` | Broadcast a message. |
| `actionbar` | `"[actionbar] <green>Welcome!"` | Send action-bar text. |
| `title` | `"[title] Welcome;Enjoy;10;60;10"` | Show title and subtitle; timing values are ticks. |
| `kick` | `"[kick] <gray>Goodbye"` | Kick the player with a reason. |
| `sound` | `"[sound] entity.experience_orb.pickup 1 1"` | Play a sound; arguments are sound, volume, pitch. |
| `server` | `"[server] lobby"` | Connect through configured BungeeCord/Velocity-compatible messaging; `[connect]` is an alias. |
| `teleport` | `"[teleport] world 100 64 200 90 0"` | Teleport to world, X, Y, Z, yaw, pitch. |
| `open` | `"[open] games"` | Open another menu; `[menu]` is an alias. |
| `refresh` | `"[refresh]"` | Reopen the current menu. |
| `close` | `"[close]"` | Close the dialog and leave the camera scene. |
| `delay` | `"[delay] 40"` | Wait 40 ticks (about 2 seconds) before continuing. |

## Example: join a proxy server

```yaml
actions:
  - "[sound] entity.experience_orb.pickup 1 1"
  - "[message] <green>Connecting to <white>Battle Royale</white>..."
  - "[delay] 10"
  - "[server] battleroyale"
```

## Notes

- Placeholders are resolved before the action runs.
- An action without a tag defaults to a player command, according to the plugin's documented behavior.
- `[delay]` delays the following action sequence.
- Proxy server switching requires a correctly configured proxy and messaging channel.
- Verify every action in a test server before using it on a live network.
