# Marvin's AIRA Refactored - Folder Structure by Function

This document categorizes the folders in the Marvin's AIRA Refactored codebase by their primary function to help identify what is core FFB functionality versus UI, services, and other features.

## Core FFB Functions

### `Components/` (Core FFB Processing)
**Primary FFB Components:**
- `RacingWheel.cs` - Main FFB processing logic (algorithms, interpolation, effects, output)
- `DirectInput.cs` - DirectInput interface for force feedback device communication
- `Simulator.cs` - iRacing SDK integration and telemetry reading
- `SteeringEffects.cs` - Understeer/oversteer/seat-of-pants effect calculations
- `LFE.cs` - Low Frequency Effects processing (audio-based effects)
- `Pedals.cs` - Pedal force feedback effects (Simagic HPR integration)

**Supporting FFB Components:**
- `Telemetry.cs` - Memory-mapped file for telemetry output (SimHub integration)
- `RecordingManager.cs` - Records FFB data for algorithm preview
- `Graph.cs` - Real-time FFB signal visualization

## Adapters/Interfaces

### `Components/Simulator.cs`
- iRacing SDK adapter (`IRSDKSharper` wrapper)
- Reads telemetry data (steering wheel torque, speed, accelerations, etc.)
- Provides normalized telemetry interface

### `Components/DirectInput.cs`
- DirectInput API adapter for racing wheel communication
- Device enumeration and force feedback effect management
- Wheel position/velocity reading

### `PInvoke/`
- Windows API interop declarations
- `User32.cs` - Windows user interface APIs
- `DWMAPI.cs` - Desktop Window Manager APIs
- `UXTheme.cs` - Windows theme APIs
- `WinMM.cs` - Windows Multimedia APIs

## Services/Infrastructure

### `Components/` (Infrastructure Services)
- `Logger.cs` - Logging service
- `MultimediaTimer.cs` - High-precision timing service
- `HidHotPlugMonitor.cs` - USB device hot-plug detection
- `SettingsFile.cs` - Settings persistence
- `TopLevelWindow.cs` - Window management

### `DataContext/`
- `DataContext.cs` - Application-wide data context
- `Settings.cs` - Settings data model
- `Localization.cs` - Multi-language support
- `Context.cs`, `ContextSettings.cs`, `ContextSwitches.cs` - Context management

## UI Components

### `Windows/`
- `MainWindow.xaml/cs` - Main application window
- `HelpWindow.xaml/cs` - Help documentation window
- `ErrorWindow.xaml/cs` - Error display window
- `GripOMeterWindow.xaml/cs` - Grip meter overlay
- `CursorCountdownOverlay.xaml/cs` - Countdown overlay
- `SpeechToTextWindow.xaml/cs` - Speech-to-text window
- `NewVersionAvailableWindow.xaml/cs` - Update notification
- `UpdateButtonMappingsWindow.xaml/cs` - Button mapping editor
- `UpdateContextSwitchesWindow.xaml/cs` - Context switch editor
- `RunInstallerWindow.xaml/cs` - Installer runner

### `Pages/`
- `RacingWheelPage.xaml/cs` - FFB settings and configuration page
- `SteeringEffectsPage.xaml/cs` - Steering effects calibration and settings
- `PedalsPage.xaml/cs` - Pedal effects configuration
- `SimulatorPage.xaml/cs` - Simulator connection settings
- `GraphPage.xaml/cs` - FFB signal graph display
- `SoundsPage.xaml/cs` - Sound effects configuration
- `SpeechToTextPage.xaml/cs` - Speech-to-text settings
- `TradingPaintsPage.xaml/cs` - Trading Paints integration settings
- `WindPage.xaml/cs` - Wind controller settings
- `AdminBoxxPage.xaml/cs` - AdminBoxx device settings
- `AppSettingsPage.xaml/cs` - General application settings
- `DebugPage.xaml/cs` - Debug information display
- `HelpPage.xaml/cs` - Help content
- `ContributePage.xaml/cs` - Contribution information
- `DonatePage.xaml/cs` - Donation page

### `Controls/`
- Custom WPF controls (buttons, sliders, combo boxes, text boxes, switches, knobs, etc.)
- All files prefixed with `Maira*` (e.g., `MairaButton.xaml`, `MairaSlider.xaml`)

### `Themes/`
- WPF theme/style definitions
- `Generic.xaml` - Default theme
- Control-specific theme files

### `Artwork/`
- Application icons, button images, flags, and other visual assets
- `AppIcon/` - Application icon variations
- `Buttons/` - Button images
- `Flags/` - Country flag images
- `Source/` - Source artwork files

### `Fonts/`
- Custom font files (Aptos Narrow)

## Utilities/Helper Classes

### `Classes/`
- `MathZ.cs` - Math utilities (interpolation, curves, compression, etc.)
- `Serializer.cs` - Settings serialization
- `GraphBase.cs` - Graph rendering base class
- `Recording.cs`, `RecordingData.cs` - Recording data structures
- `CachedSound.cs`, `CachedSoundPlayer.cs` - Audio caching
- `LogitechGSDK.cs` - Logitech SDK wrapper
- `UsbSerialPortHelper.cs` - USB serial communication
- `ChromeLauncher.cs`, `ChromeSTTBridge.cs` - Chrome integration
- `TradingPaintsXML.cs` - Trading Paints XML parsing
- `ButtonMappings.cs` - Button mapping utilities
- `HelpIconVisibilityConverter.cs` - UI converter
- `HelpService.cs` - Help system service
- `Color.cs` - Color utilities
- `Misc.cs` - Miscellaneous utilities
- `TextBoxBehaviors.cs` - Text box behaviors

## Non-FFB Features

### `Components/` (Additional Features)
- `AudioManager.cs` - Audio playback management
- `Sounds.cs` - Sound effect system
- `SpeechToText.cs` - Speech-to-text functionality
- `TradingPaints.cs` - Trading Paints integration
- `CloudService.cs` - Cloud service integration
- `ChatQueue.cs` - Chat message queue
- `AdminBoxx.cs` - AdminBoxx device support
- `Wind.cs` - Wind controller support
- `Debug.cs` - Debug information management
- `VirtualJoystick.cs` - Virtual joystick (vJoy) for calibration

## Build/Deployment

### `InnoSetup/`
- Installer scripts and resources
- `MarvinsAIRA.iss` - Main installer script
- `Calibration/`, `Recordings/`, `SimHub/`, `Sounds/`, `STT/` - Installer resources

### `bin/`, `obj/`
- Build output directories (compiled binaries and intermediate files)

## Documentation/Resources

### `Help/`
- Help documentation files

### `html/`, `latex/`
- Generated documentation (likely from Doxygen)

### `Notes/`
- Development notes (`build_notes.txt`, `vjoy_notes.txt`)

### `Doxyfile`
- Doxygen configuration for API documentation generation

## External Hardware/Devices

### `AdminBoxx/`
- AdminBoxx device firmware/support files
- `boot.py`, `code.py` - CircuitPython files

### `Arduino/`
- Arduino code for wind controller
- `Wind/` - Wind controller firmware

### `KiCad/`
- PCB design files
- `Wind Controller/`, `Wind Pod/` - Hardware designs

## Other

### `Translate/`
- Translation/localization files

### `Viewers/`
- Unknown purpose (needs investigation)

### `Website/`
- Website-related files

### Root Files
- `App.xaml/cs` - Application entry point
- `AssemblyInfo.cs` - Assembly metadata
- `GlobalSuppressions.cs` - Code analysis suppressions
- `MarvinsAIRARefactored.csproj` - Project file
- `MarvinsAIRARefactored.sln` - Solution file
- `README.md` - Project readme
- `LICENSE` - License file
- `LogitechSteeringWheelEnginesWrapper.dll` - Logitech SDK wrapper
- `vJoyInterface.dll`, `vJoyInterfaceWrap.dll` - vJoy library files
- `ZipFilesForChatGPT.ps1` - Utility script

## Summary

**Core FFB (Essential for FFB functionality):**
- `Components/RacingWheel.cs`
- `Components/DirectInput.cs`
- `Components/Simulator.cs`
- `Components/SteeringEffects.cs`
- `Components/LFE.cs`
- `Components/Pedals.cs`
- `Components/Telemetry.cs`
- `Classes/MathZ.cs` (math utilities used by FFB)

**Adapters (Game/Wheel interfaces):**
- `Components/Simulator.cs` (iRacing adapter)
- `Components/DirectInput.cs` (wheel adapter)
- `PInvoke/` (Windows API interop)

**UI (Can be replaced/ignored for core FFB):**
- `Windows/`
- `Pages/`
- `Controls/`
- `Themes/`
- `Artwork/`
- `Fonts/`

**Services/Infrastructure (Supporting but not core FFB):**
- `Components/Logger.cs`
- `Components/MultimediaTimer.cs`
- `Components/HidHotPlugMonitor.cs`
- `Components/SettingsFile.cs`
- `DataContext/`

**Non-FFB Features (Can be ignored for FFB-only implementation):**
- `Components/AudioManager.cs`
- `Components/Sounds.cs`
- `Components/SpeechToText.cs`
- `Components/TradingPaints.cs`
- `Components/CloudService.cs`
- `Components/ChatQueue.cs`
- `Components/AdminBoxx.cs`
- `Components/Wind.cs`
- `Components/VirtualJoystick.cs` (used for calibration only)

