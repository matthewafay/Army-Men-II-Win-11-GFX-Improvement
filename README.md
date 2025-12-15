# Army Men 2 Configuration Tool

A PowerShell script that automatically configures Army Men 2 (Steam version) for optimal Windows 11 compatibility using cnc-ddraw wrapper.

## Overview

This tool automates the complex setup process required to run Army Men 2 on modern Windows systems by:

- **Auto-detecting** your screen resolution
- **Locating** your Steam installation and game files
- **Installing** cnc-ddraw wrapper for windowed mode compatibility
- **Configuring** DirectDraw interception to prevent crashes and resolution changes

## Features

- ✅ **Automatic Resolution Detection** - Detects your primary monitor's resolution using multiple fallback methods
- ✅ **Steam Integration** - Automatically finds Steam installation and library folders
- ✅ **Game Discovery** - Locates Army Men 2 installation across multiple Steam libraries
- ✅ **cnc-ddraw Integration** - Downloads and installs cnc-ddraw wrapper for modern compatibility
- ✅ **Windowed Mode** - Forces the game to run in a resizable window instead of changing screen resolution
- ✅ **DirectPlay Support** - Guides users through DirectPlay installation when needed
- ✅ **Comprehensive Testing** - Includes extensive unit tests and property-based testing

## Quick Start

1. **Prerequisites**: Ensure you have Army Men 2 installed via Steam
2. **Run the script**: Execute `Configure-ArmyMen2.ps1` in PowerShell
3. **Follow the output**: The script will display progress and results for each configuration step

```powershell
.\Configure-ArmyMen2.ps1
```

## What It Does

### Phase 1: Resolution Detection
- Detects your primary monitor's resolution using Windows Forms API
- Falls back to WMI queries if the primary method fails
- Validates resolution values are within acceptable ranges

### Phase 2: Steam Location
- Searches Windows Registry for Steam installation path
- Parses Steam's `libraryfolders.vdf` to find all library locations
- Handles both 32-bit and 64-bit registry locations

### Phase 3: Game Discovery
- Searches all Steam libraries for Army Men 2 (App ID: 299220)
- Parses Steam manifest files to locate game installation directory
- Verifies game executable (`AM2.exe`) exists

### Phase 4: cnc-ddraw Installation
- Downloads the latest cnc-ddraw wrapper from GitHub
- Backs up the original ddraw.dll file
- Installs cnc-ddraw with windowed mode configuration
- Configures GDI renderer for maximum compatibility

### Phase 5: Game Configuration
- Creates or updates game configuration files
- Forces windowed mode to prevent screen resolution changes
- Disables DirectDraw acceleration that causes crashes
- Creates backup launcher for troubleshooting

## Requirements

- **PowerShell 5.1** or later
- **Windows 10/11** (tested on Windows 11)
- **Army Men 2** installed via Steam
- **Internet connection** (to download cnc-ddraw)
- **DirectPlay Windows feature** (script will prompt for installation if needed)

## Testing

The project includes comprehensive test coverage using Pester:

```powershell
# Run all tests
Invoke-Pester .\tests\Configure-ArmyMen2.Tests.ps1

# Run specific test categories
Invoke-Pester .\tests\Configure-ArmyMen2.Tests.ps1 -Tag "Unit"
Invoke-Pester .\tests\Configure-ArmyMen2.Tests.ps1 -Tag "Property"
```

### Test Coverage
- **Unit Tests**: Individual function testing with mocked dependencies
- **Property-Based Tests**: Validates behavior across random input ranges
- **Integration Tests**: End-to-end workflow validation

## Troubleshooting

### Common Issues

**"Steam installation not found"**
- Ensure Steam is installed and has been run at least once
- Check that Steam appears in Windows "Add or Remove Programs"

**"Army Men 2 not found"**
- Verify the game is installed via Steam
- Check that the game appears in your Steam library
- Try running Steam as administrator and verify game files

**"Failed to detect screen resolution"**
- Update your display drivers
- Ensure your monitor is properly connected and recognized by Windows
- Try running the script as administrator

**"DirectPlay required" popup**
- Click "Install this feature" when Windows prompts
- Restart your computer after installation
- DirectPlay is required for the game's networking code

**"Black screen in windowed mode"**
- The script automatically configures cnc-ddraw with GDI renderer
- If issues persist, try editing `ddraw.ini` in the game folder
- Change `renderer=gdi` to `renderer=opengl` or `renderer=direct3d9`

### Manual Configuration

If the script fails, you can manually set up cnc-ddraw:

1. **Download cnc-ddraw**: Get the latest version from GitHub (FunkyFr3sh/cnc-ddraw)
2. **Install**: Copy `ddraw.dll` to the game directory
3. **Configure**: Create `ddraw.ini` with these settings:
   ```ini
   [ddraw]
   windowed=true
   fullscreen=false
   renderer=gdi
   nonexclusive=true
   singlecpu=true
   ```
4. **DirectPlay**: Enable via Windows Features → Legacy Components → DirectPlay

## Architecture

The script is organized into modular components:

- **Resolution Detection Module**: Multi-method screen resolution detection
- **Steam Locator Module**: Registry-based Steam installation discovery
- **Game Finder Module**: Steam library parsing and game location
- **Compatibility Configurator Module**: Windows compatibility registry management
- **Game Configurator Module**: INI file configuration management
- **Output Functions**: Formatted status reporting and summary generation

## After Installation

**First Launch:**
1. Run the configuration script: `.\Configure-ArmyMen2.ps1`
2. Launch Army Men 2 from Steam
3. If prompted for DirectPlay, click "Install this feature" and restart
4. Game should now open in a resizable window

**What You Get:**
- ✅ Windowed mode (no more screen resolution changes)
- ✅ Modern rendering via cnc-ddraw wrapper  
- ✅ Stable performance on Windows 11
- ✅ Resizable game window
- ✅ No crashes or compatibility issues

## Contributing

This project uses property-based testing to ensure reliability across different system configurations. When contributing:

1. Add unit tests for new functions
2. Include property-based tests for functions with variable inputs
3. Update documentation for any new features
4. Ensure all tests pass before submitting changes

## License

This project is provided as-is for educational and personal use. Army Men 2 is a trademark of its respective owners.