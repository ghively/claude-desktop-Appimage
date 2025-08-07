# Claude Desktop for Linux

This project provides universal build scripts to run Claude Desktop natively on any Linux distribution. It repackages the official Windows application into a Linux-compatible portable AppImage.

**Note:** This is an unofficial build script. For official support, please visit [Anthropic's website](https://www.anthropic.com). For issues with the build script or Linux implementation, please [open an issue](https://github.com/aaddrick/claude-desktop-debian/issues) in this repository.

## Features

- **Native Linux Support**: Run Claude Desktop without virtualization or Wine
- **MCP Support**: Full Model Context Protocol integration  
  Configuration file location: `~/.config/Claude/claude_desktop_config.json`
- **System Integration**: 
  - Global hotkey support (Ctrl+Alt+Space)
  - System tray integration
  - Desktop environment integration

### Screenshots

![Claude Desktop running on Linux](https://github.com/user-attachments/assets/93080028-6f71-48bd-8e59-5149d148cd45)

![Global hotkey popup](https://github.com/user-attachments/assets/1deb4604-4c06-4e4b-b63f-7f6ef9ef28c1)

![System tray menu on KDE](https://github.com/user-attachments/assets/ba209824-8afb-437c-a944-b53fd9ecd559)

## Installation

### Using Pre-built Releases

Download the latest `.AppImage` from the [Releases page](https://github.com/aaddrick/claude-desktop-debian/releases).

### Building from Source

#### Prerequisites

- Any modern Linux distribution
- Git
- Basic build tools (automatically installed by the script)

### Supported Distributions

The build script automatically detects and supports the following package managers:
- **DNF/YUM**: Fedora, RHEL, CentOS, Rocky Linux, AlmaLinux
- **APT**: Debian, Ubuntu, Linux Mint, Pop!_OS, Elementary OS
- **Zypper**: openSUSE, SUSE Linux Enterprise
- **Pacman**: Arch Linux, Manjaro, EndeavourOS
- **APK**: Alpine Linux

The script has been tested on Fedora 40+ and should work on any distribution with one of the above package managers.

#### Build Instructions

```bash
# Clone the repository
git clone https://github.com/aaddrick/claude-desktop-debian.git
cd claude-desktop-debian

# Build an AppImage
./build.sh

# Build with custom options
./build.sh --clean no  # Keep intermediate files
```

**Note**: The script will automatically detect your distribution and install required dependencies using your system's package manager.

#### Installing the Built Package

**For AppImages:**

**Note:** AppImage login requires proper desktop integration. Use [AppImageLauncher](https://github.com/TheAssassin/AppImageLauncher) or manually install the provided `.desktop` file to `~/.local/share/applications/.`

## Configuration

### MCP Configuration

Model Context Protocol settings are stored in:
```
~/.config/Claude/claude_desktop_config.json
```

### Application Logs

Runtime logs are available at:
```
$HOME/claude-desktop-launcher.log
```

## Uninstallation


```bash
# Remove package
sudo dpkg -r claude-desktop

# Remove package and configuration
sudo dpkg -P claude-desktop
```

**For AppImages:**
1. Delete the `.AppImage` file
2. Remove the `.desktop` file from `~/.local/share/applications/`
3. If using AppImageLauncher, use its uninstall option

**Remove user configuration (both formats):**
```bash
rm -rf ~/.config/Claude
```

## Troubleshooting

### Window Scaling Issues

If the window doesn't scale correctly on first launch:
1. Right-click the Claude Desktop tray icon
2. Select "Quit" (do not force quit)
3. Restart the application

This allows the application to save display settings properly.

### AppImage Sandbox Warning

AppImages run with `--no-sandbox` due to electron's chrome-sandbox requiring root privileges for unprivileged namespace creation. This is a known limitation of AppImage format with Electron applications.

For enhanced security, consider running the AppImage within a separate sandbox (e.g., bubblewrap)

## Technical Details

### How It Works

Claude Desktop is an Electron application distributed for Windows. This project:

1. Downloads the official Windows installer
2. Extracts application resources
3. Replaces Windows-specific native modules with Linux-compatible implementations
4. Repackages as either:
   - **Debian package**: Standard system package with full integration
   - **AppImage**: Portable, self-contained executable

### Build Process

The build script (`build.sh`) handles:
- Automatic distribution detection and package manager identification
- Dependency installation via dnf, yum, apt, zypper, pacman, or apk
- Resource extraction from Windows installer (using 7-zip)
- Icon processing for Linux desktop standards
- Native module replacement for Linux compatibility
- Package generation (`.AppImage`)
- Architecture detection (x86_64/amd64 and arm64)

### Dependencies

The script automatically installs these dependencies based on your distribution:
- **7-zip**: For extracting Windows installer files
- **wget**: For downloading the Claude installer
- **icoutils**: For extracting and converting icons (wrestool, icotool)
- **ImageMagick**: For image processing (convert command)
- **Node.js 20+**: Automatically downloaded if not present or outdated

### Updating for New Releases

The script automatically detects system architecture and downloads the appropriate version. If Claude Desktop's download URLs change, update the `CLAUDE_DOWNLOAD_URL` variables in `build.sh`.

## Platform Support

### Tested Distributions
- Fedora 40+
- Ubuntu 22.04+
- Debian 12+
- Arch Linux
- openSUSE Tumbleweed

### Package Manager Compatibility
The build script automatically detects and uses:
1. **dnf** (Fedora, RHEL 8+, Rocky, AlmaLinux)
2. **yum** (older RHEL, CentOS)
3. **apt** (Debian, Ubuntu, Mint)
4. **zypper** (openSUSE, SUSE)
5. **pacman** (Arch, Manjaro)
6. **apk** (Alpine)

If your distribution uses a different package manager, you can manually install the required tools:
- 7z (or 7za, 7zr)
- wget
- wrestool and icotool (usually in icoutils package)
- convert (usually in ImageMagick package)

## Acknowledgments

This project was inspired by [k3d3's claude-desktop-linux-flake](https://github.com/k3d3/claude-desktop-linux-flake) and their [Reddit post](https://www.reddit.com/r/ClaudeAI/comments/1hgsmpq/i_successfully_ran_claude_desktop_natively_on/) about running Claude Desktop natively on Linux.

Special thanks to:
- **k3d3** for the original NixOS implementation and native bindings insights
- **[emsi](https://github.com/emsi/claude-desktop)** for the title bar fix and alternative implementation approach

For NixOS users, please refer to [k3d3's repository](https://github.com/k3d3/claude-desktop-linux-flake) for a Nix-specific implementation.

## License

The build scripts in this repository are dual-licensed under:
- MIT License (see [LICENSE-MIT](LICENSE-MIT))
- Apache License 2.0 (see [LICENSE-APACHE](LICENSE-APACHE))

The Claude Desktop application itself is subject to [Anthropic's Consumer Terms](https://www.anthropic.com/legal/consumer-terms).

## Contributing

Contributions are welcome! By submitting a contribution, you agree to license it under the same dual-license terms as this project.