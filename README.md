# Sidenote, site compatibility data

This repository holds `selectors.json`, the file the [Sidenote](https://apps.apple.com) macOS app uses to read each supported chat site's web page (where the prompts, replies and text box are). The app downloads it once a day; when a site redesigns its page, updating this file fixes every installed copy without an app update.

The file contains no user data and the download carries none. It is a plain, public, read‑only file.

## Updating it

1. Find the new element names with the app's **Copy diagnostics** button (Settings off → bottom bar) on the affected site. Selectors that match `0` are the stale ones.
2. Edit the site's entry in `selectors.json`. Each field is a comma‑separated list tried in order; put the new selector **first** and keep the old ones as fallbacks.
3. Increase `"version"` by one. The app only adopts a file with a higher version.
4. Commit to `main`. The app picks it up within 24 hours, or immediately via *Settings › General › Check now*.
5. Copy the same file into the app project (`Sidenote/selectors.json`) so the next build ships it as the default.

### Fields

| Field | What it must match |
|---|---|
| `hosts` | Hostnames on which hidden mode is applied (sign‑in hosts are left alone). |
| `user` | One element per prompt from the user. |
| `assistant` | One element per reply. If this stops matching, the app infers replies from the layout around the prompts. |
| `composer` | The site's text box (textarea or contenteditable). |
| `send` | The send button. If absent, Enter is pressed. |
| `streaming` | Something visible only while a reply is being generated (usually the Stop button). |
| `newChatURL` | Where a new conversation starts. |

Validate before committing: `python3 -c "import json; json.load(open('selectors.json'))"`.

## Also here

- `docs/privacy.md` — the app's privacy policy (published via GitHub Pages).
- `docs/support.md` — support page.
