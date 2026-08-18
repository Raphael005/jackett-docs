# Jackett Installation on macOS

## Installation

1. **Download the pre-built release:**
   ```bash
   cd ~/Downloads
   curl -L -o Jackett.Binaries.macOS.tar.gz https://github.com/Jackett/Jackett/releases/latest/download/Jackett.Binaries.macOS.tar.gz
   tar -xzf Jackett.Binaries.macOS.tar.gz
   cd Jackett
   ```

2. **Install as a launch agent:**
   ```bash
   sudo ./install_service_macos
   ```

3. **If the agent fails to load**, reload it manually:
   ```bash
   launchctl bootout gui/$(id -u) ~/Library/LaunchAgents/org.user.Jackett.plist 2>/dev/null
   launchctl bootstrap gui/$(id -u) ~/Library/LaunchAgents/org.user.Jackett.plist
   ```

4. **Approve in System Settings** (if prompted):
   - Go to **System Settings → Privacy & Security**
   - Look for a message about "jackett" being blocked
   - Click **Allow Anyway**
   - Restart the service:
     ```bash
     launchctl kickstart -k gui/$(id -u)/org.user.Jackett
     ```

## Access

- **Web UI:** http://localhost:9117

## Service Management

| Action | Command |
|--------|---------|
| Check status | `launchctl list \| grep -i jackett` |
| Restart | `launchctl kickstart -k gui/$(id -u)/org.user.Jackett` |
| Stop | `launchctl bootout gui/$(id -u) ~/Library/LaunchAgents/org.user.Jackett.plist` |
| Start | `launchctl bootstrap gui/$(id -u) ~/Library/LaunchAgents/org.user.Jackett.plist` |

## Uninstall

```bash
cd ~/Downloads/Jackett
sudo ./uninstall_jackett_macos
```

## File Locations

- **Installation:** `~/Downloads/Jackett/`
- **Config:** `~/.config/Jackett/`
- **Launch Agent:** `~/Library/LaunchAgents/org.user.Jackett.plist`

## Building from Source

### Requirements

- .NET SDK 8.0+ or 9.0+

### Build

```bash
cd /path/to/Jackett-master/src
dotnet build Jackett.sln
```

### Running Tests

```bash
dotnet test Jackett.sln
```

#### Test Environment Requirements

Some tests require additional dependencies:

| Dependency | Required For | Install |
|------------|--------------|--------|
| Mono | .NET Framework 4.7.1 tests | `brew install mono` |
| Chrome | Integration tests | Download from Google |
| ChromeDriver | Integration tests | `brew install chromedriver` |

#### Known Test Issues

- **Integration tests**: Require Chrome and ChromeDriver in PATH
- **Net471 tests**: Require Mono runtime on macOS
- **TestFixResultsOriginPublishDate**: May fail due to upstream date handling bug ([GitHub issue](https://github.com/Jackett/Jackett/issues))

Core unit tests (239/246) pass without these dependencies.
