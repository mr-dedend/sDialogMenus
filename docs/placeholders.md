# Placeholders and Conditions

## Built-in placeholders

| Placeholder | Value |
|---|---|
| `%player%` / `%player_name%` | Player name |
| `%uuid%` | Player UUID |
| `%world%` | Current world name |
| `%x%`, `%y%`, `%z%` | Player block coordinates |
| `%online%` | Online player count |
| `%max_players%` | Server maximum player count |
| `%server_<name>%` | Player count reported for a proxy server |

Placeholders can be used in titles, text, buttons, overlays, hover text, conditions, and actions where supported.

## PlaceholderAPI

When PlaceholderAPI is installed, the plugin detects it and can resolve third-party placeholders such as economy or player-stat placeholders. The actual placeholders available depend on the PlaceholderAPI expansions installed on your server.

## Proxy counts

`%server_<name>%` uses the BungeeCord plugin messaging channel. It requires a supported proxy setup and working messaging. Counts refresh periodically (documented default: 5 seconds) and may show `0` until the first response is received.

## Conditions

A `condition` controls whether an element or button is shown. An empty condition means no condition is applied. Multiple expressions can be joined with `&&`.

| Expression | Meaning |
|---|---|
| `perm:some.permission` | Player has the permission. |
| `!perm:some.permission` | Player does not have the permission. |
| `==` | Equal comparison. |
| `!=` | Not-equal comparison. |
| `>` / `<` | Greater-than / less-than numeric comparison. |
| `>=` / `<=` | Greater-or-equal / less-or-equal numeric comparison. |
| `contains` | Left value contains right value. |
| `!contains` | Left value does not contain right value. |
| `startswith` | Left value begins with right value. |

## Examples

```yaml
- type: text
  text:
    - "<green>Welcome, %player%!"
  condition: "perm:server.member"
```

```yaml
buttons:
  - label: "<gold>VIP Shop"
    condition: "perm:server.vip && %online% >= 1"
    actions:
      - "[player] vipshop"
```

```yaml
- type: text
  text:
    - "<yellow>Only show this to non-VIP players."
  condition: "!perm:server.vip"
```

Use permission nodes that actually exist on your server. Test comparisons with the values returned by your installed placeholder providers.
