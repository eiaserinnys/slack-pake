# Slack Pake

Lightweight Slack desktop client built with [Pake](https://github.com/tw93/Pake) (Tauri).

Uses the system webview instead of bundling Chromium, resulting in ~20x smaller binary and significantly lower memory usage compared to the official Electron-based Slack app.

## Download

Go to [Releases](https://github.com/eiaserinnys/slack-pake/releases) and download the latest build for your platform:

| Platform | File | Notes |
|----------|------|-------|
| macOS | `Slack.dmg` | Universal (Intel + Apple Silicon) |
| Windows | `Slack_x64.msi` | x64 installer |
| Linux | `Slack_x86_64.deb` | Debian/Ubuntu |
| Linux | `Slack_x86_64.AppImage` | Portable |

## Limitations

This is a webview wrapper around `app.slack.com`, not a full native client. Some features may behave differently:

- **Huddles/Screen sharing**: WebRTC support varies by system webview
- **Desktop notifications**: May require manual permission grant
- **File drag & drop**: Limited compared to the official app
- **Deep links** (`slack://`): Not supported

For basic messaging, channels, threads, and file sharing, it works great.

## Build Locally

Requires [Rust](https://rustup.rs/) (>= 1.85) and [Node.js](https://nodejs.org/) (>= 22).

```bash
# Install pake-cli
pnpm install -g pake-cli

# Build for your platform
pake https://app.slack.com --name Slack --new-window

# With custom icon
pake https://app.slack.com --name Slack --new-window --icon path/to/slack.icns
```

See [Pake CLI docs](https://github.com/tw93/Pake/blob/master/docs/cli-usage.md) for all options.

## Custom Icons

Place icon files in the `assets/` directory. The CI workflow will pick them up automatically:

- `assets/slack.icns` — macOS
- `assets/slack.ico` — Windows
- `assets/slack.png` — Linux (512x512 recommended)

If no custom icons are provided, Pake will auto-fetch the favicon from the website.

## How It Works

The CI workflow uses `pake-cli` to wrap `app.slack.com` into a native desktop app via Tauri 2. Tauri uses the OS-provided webview (WebView2 on Windows, WebKit on macOS/Linux) instead of shipping its own browser engine.

### Release

Tag a version to trigger the release workflow:

```bash
git tag v1.0.0
git push origin v1.0.0
```

This builds for all three platforms and uploads artifacts to GitHub Releases.

## Credits

- [Pake](https://github.com/tw93/Pake) — the engine that makes this possible
- [Tauri](https://tauri.app/) — the underlying Rust + webview framework

## License

[MIT](LICENSE)
