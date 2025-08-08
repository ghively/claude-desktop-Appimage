# Claude Desktop for Linux

This project provides a universal build script to run Claude Desktop natively on any Linux distribution. It repackages the official Windows application into a Linux-compatible portable AppImage.

**Note:** This is an unofficial build script. For official support, please visit [Anthropic's website](https://www.anthropic.com).

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

### Building from Source

#### Prerequisites

- Any modern Linux distribution (x86_64/amd64 or arm64)
- Git
- sudo access (for installing dependencies)
- Internet connection (for downloading Claude Desktop and dependencies)

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

# Build an AppImage (default)
./build.sh

# Build options
./build.sh --clean no     # Keep intermediate build files for debugging
./build.sh --test-flags   # Test argument parsing without building
./build.sh --help         # Show usage information
```

**Notes**: 
- The script will automatically detect your system architecture (x86_64/amd64 or arm64)
- Required dependencies will be installed automatically using your system's package manager
- The script should NOT be run as root or with sudo - it will prompt for sudo when needed

#### Running the Built AppImage

1. Make the AppImage executable:
```bash
chmod +x claude-desktop-*.AppImage
```

2. Run the AppImage:
```bash
./claude-desktop-*.AppImage
```

**For Desktop Integration:** 
- Use [AppImageLauncher](https://github.com/TheAssassin/AppImageLauncher) for automatic integration
- Or manually copy the generated `.desktop` file to `~/.local/share/applications/`
- Note: Desktop integration is required for login functionality to work properly

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

1. Delete the `.AppImage` file
2. Remove the `.desktop` file from `~/.local/share/applications/` (if manually installed)
3. If using AppImageLauncher, use its uninstall option

**Remove user configuration and logs:**
```bash
rm -rf ~/.config/Claude
rm -f ~/claude-desktop-launcher.log
```

## Troubleshooting

### Window Scaling Issues

If the window doesn't scale correctly on first launch:
1. Right-click the Claude Desktop tray icon
2. Select "Quit" (do not force quit)
3. Restart the application

This allows the application to save display settings properly.

### AppImage Sandbox Warning

AppImages run with `--no-sandbox` due to Electron's chrome-sandbox requiring root privileges for unprivileged namespace creation. This is a known limitation of the AppImage format with Electron applications.

For enhanced security, consider running the AppImage within a separate sandbox (e.g., bubblewrap, firejail)

## Technical Details

### How It Works

Claude Desktop is an Electron application distributed for Windows. This project:

1. Downloads the official Windows installer
2. Extracts application resources using 7-zip
3. Replaces Windows-specific native modules with Linux-compatible stub implementations
4. Modifies the application to enable the title bar on Linux
5. Repackages as an **AppImage**: Portable, self-contained executable with bundled Electron

### Build Process

The build script (`build.sh`) handles:
- Architecture detection (x86_64/amd64 and arm64)
- Automatic distribution detection and package manager identification
- Dependency installation via dnf, yum, apt, zypper, pacman, or apk
- Node.js 20+ installation (downloads locally if not present)
- Electron and asar tool installation
- Resource extraction from Windows installer (using 7-zip)
- Icon processing for Linux desktop standards
- Native module replacement with Linux-compatible stubs
- Title bar fix for Linux windowing systems
- AppImage generation with proper desktop integration

### Dependencies

The script automatically installs these dependencies based on your distribution:
- **7-zip**: For extracting Windows installer files
- **wget**: For downloading the Claude installer
- **icoutils**: For extracting and converting icons (wrestool, icotool)
- **ImageMagick**: For image processing (convert command)
- **Node.js 20+**: Automatically downloaded if not present or outdated

### Key Features

- **Wayland Support**: Automatically detects and enables Wayland-specific flags
- **System Tray Integration**: Full system tray support with context menu
- **Global Hotkey**: Ctrl+Alt+Space for quick access
- **URL Scheme Handler**: Registers `claude://` URL scheme for deep linking
- **Logging**: Application logs to `~/claude-desktop-launcher.log` for debugging

### Updating for New Releases

The script automatically detects system architecture and downloads the appropriate version. If Claude Desktop's download URLs change, update the `CLAUDE_DOWNLOAD_URL` variables in `build.sh`:
- Line 17 for x86_64/amd64
- Line 27 for arm64

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

## License

The build scripts in this repository are dual-licensed under:
- MIT License (see [LICENSE-MIT](LICENSE-MIT))
- Apache License 2.0 (see [LICENSE-APACHE](LICENSE-APACHE))

The Claude Desktop application itself is subject to [Anthropic's Consumer Terms](https://www.anthropic.com/legal/consumer-terms).

## Known Limitations

- AppImage runs with `--no-sandbox` flag (Electron/AppImage limitation)
- Login requires proper desktop integration
- Initial window scaling may require restart on some systems

## Contributing

Contributions are welcome! By submitting a contribution, you agree to license it under the same dual-license terms as this project.

### Reporting Issues

- For Claude Desktop functionality issues: Contact [Anthropic support](https://www.anthropic.com)
