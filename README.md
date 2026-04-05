<div align="center">

# Zapret Discord YouTube Linux (english translation)

### A tool to bypass cencorship on Linux






[![GitHub forks](https://img.shields.io/github/forks/Sergeydigl3/zapret-discord-youtube-linux?style=social)](https://github.com/Sergeydigl3/zapret-discord-youtube-linux/network/members)

</div>

---

<div align="center">

[Quick Start](#быстрый-старт) • [How to Use](#использование) • [Autostart](#автозагрузка) • [Support](#поддержка-и-помощь)


</div>

## Quick Start

```bash
git clone https://github.com/Sergeydigl3/zapret-discord-youtube-linux.git
cd zapret-discord-youtube-linux

./service.sh download-deps --default
./service.sh
```

The script will prompt you to select an action: start, manage the service, or configure settings.

>  **Passwordless operation:** `./service.sh setup-permissions` — configures NOPASSWD for nft/nfqws

---

**Requirements:**
- Works only with **nftables**
- Supported architectures: **x86_64, ARM, MIPS, etc.** (automatic detection)

---

## About versions

The adapter uses the following by default:
- **nfqws**: v72.9 (recommended version, defined in `src/lib/constants.sh` as `ZAPRET_RECOMMENDED_VERSION`)
- **Strategies**: [commit cb9aed09449e1c51a9108c7989717c7c98a14301] (https://github.com/Flowseal/zapret-discord-youtube/commit/cb9aed09449e1c51a9108c7989717c7c98a14301) (defined in `src/lib/constants.sh` as `MAIN_REPO_REV`)

You can change the versions:
- Interactively: `./service.sh download-deps` (select from available versions)
- Directly: `./service.sh download-deps -z v72.9 -s main`
- In the code: edit the constants in `src/lib/constants.sh`

If the current version isn’t working, try the [stable releases](https://github.com/Sergeydigl3/zapret-discord-youtube-linux/releases).
**Third-party projects:**
- [Version by Snowy-Fluffy](https://github.com/Snowy-Fluffy/zapret.installer)
---

# Usage

## Interactive mode

```bash
./service.sh
```

The menu offers:
1. **Run** — interactive selection of interface, gamefilter and bypass strategy; launch in the current terminal
2. **Service management** — install/uninstall/restart the system service
3. **Change configuration** — edit `conf.env`

## Configuration (conf.env)

Create a file called `conf.env`:

```bash
strategy=general.bat
interface=enp0s3
gamefilter=true
```

## CLI Management

### Basic Commands

```bash
./service.sh --help  # display command help
```

### Dependency Management

```bash
# Download nfqws and strategies (interactive version selection)
./service.sh download-deps

# Download recommended versions (non-interactive)
./service.sh download-deps --default

# Download specific versions
./service.sh download-deps -z v72.9 -s main

# List available strategies
./service.sh strategy list
```

### Running zapret

```bash
# Interactive mode (prompt for parameters)
./service.sh run

# Load from a configuration file
./service.sh run --config conf.env

# Direct parameters
./service.sh run -s general.bat -i enp0s3
./service.sh run -s general.bat -i enp0s3 -g  # with gamefilter
```

### Managing the system service

```bash
# Interactive service management menu
./service.sh service

# Install and start the service
./service.sh service install

# Show status
./service.sh service status

# Start/stop/restart
./service.sh service start
./service.sh service stop
./service.sh service restart

# Remove service
./service.sh service remove
```

### Configuration management

```bash
# Show current configuration
./service.sh config show

# Interactive editing
./service.sh config edit

# Set configuration directly
./service.sh config set general.bat
./service.sh config set general.bat enp0s3 -g  # with gamefilter
./service.sh config set discord -n             # without restarting the service
```

### Creating a shortcut in the Applications menu

```bash
# Create a shortcut in the Applications menu (for GUI launch)
./service.sh desktop install

# Remove the shortcut from the Applications menu
./service.sh desktop remove
```

Once the shortcut has been installed, you will be able to launch zapret from your system’s applications menu (under the ‘Network’ or ‘System’ category).

### Utilities

```bash
# Stop nfqws and flush nftables
./service.sh kill
```

---

## Automatic strategy selection

```bash
./auto_tune_youtube.sh
```

The script automatically:
1. Iterates through strategies in `/custom-strategies` and `/zapret-latest` (starting with `general`)
2. Tests access to YouTube
3. Saves the results to `auto_tune_youtube_results.txt`
4. Offers to run or save a working strategy to `conf.env`

> This feature is experimental; may not be reliable.

---

## Autostart (system service)

```bash
# Via the CLI
./service.sh service install

# Or via the interactive menu
./service.sh
# -> select ‘2. Service management’ -> ‘1. Install and start service’
```

Script:
- Checks `conf.env` (if empty, it will prompt for parameters interactively)
- Creates a service for autostart (supports systemd, OpenRC, runit, s6, dinit)
- Uses values from `conf.env`

<details>
<summary>For systemd systems</summary>

```bash
systemctl status zapret_discord_youtube.service
```

View service logs:

```bash
journalctl -u zapret_discord_youtube.service
```

</details>

<details>
<summary>For OpenRC systems</summary>

You can view the service status using the command:

```bash
rc-service zapret_discord_youtube status
```

View service logs:

```bash
rc-service zapret_discord_youtube logs
```

</details>

<details>
<summary>For runit systems</summary>

You can view the service status using the command:

```bash
sv status zapret_discord_youtube
```

View service logs:

```bash
tail -f /var/log/zapret_discord_youtube/current
```

</details>

<details>
<summary>For s6 systems</summary>

You can view the service status using the command:

```bash
s6-svstat /var/service/zapret_discord_youtube
```

View service logs:

```bash
tail -f /var/log/zapret_discord_youtube/current
```

</details>

<details>
<summary>For dinit systems</summary>

You can view the service status using the command:

```bash
dinitctl status zapret_discord_youtube
```

View the service logs:

```bash
dinitctl log zapret_discord_youtube
```

</details>

---

## Support and help

> [!IMPORTANT]
> This is an ADAPTER! It does not guarantee that the policies will unblock everything.

### If nothing works

**Before creating an Issue or Discussion:**

1. Check the [Issues in the strategies repository](https://github.com/Flowseal/zapret-discord-youtube/issues) — the issue may already be discussed there
2. Try other strategies or use [automatic selection](#automatic-strategy-selection)
3. Check the [Discussions](https://github.com/Flowseal/zapret-discord-youtube/discussions) — working solutions are discussed there

### When to create an Issue/Discussion

**When to post in [Issues](https://github.com/Sergeydigl3/zapret-discord-youtube-linux/issues):**
- Errors in the **adapter script**
- Questions about the **adapter script**
- Suggestions to add a strategy to custom-strategies

**When to post in [Discussions](https://github.com/Sergeydigl3/zapret-discord-youtube-linux/discussions):**
- YouTube or another website isn’t working (after checking the Flowseal repository)
- Searching for working strategies
- Sharing experiences

## Contributors

<div align="center">

**Thank you to everyone who helps improve the project!** 🎉

<a href="https://github.com/Sergeydigl3/zapret-discord-youtube-linux/graphs/contributors">
  <img src="https://contrib.rocks/image?repo=Sergeydigl3/zapret-discord-youtube-linux" alt="Contributors" />
</a>

Would you like to see your name here? Make a [Pull Request](https://github.com/Sergeydigl3/zapret-discord-youtube-linux/pulls)!

</div>

---

<div align="center">

[![Star History Chart](https://api.star-history.com/svg?repos=Sergeydigl3/zapret-discord-youtube-linux&type=Date)](https://star-history.com/#Sergeydigl3/zapret-discord-youtube-linux&Date)

</div>
