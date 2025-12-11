---
name: refactor-structure-wpf
overview: Define modern .NET/WPF-friendly folder layout and plan incremental moves with minimal code churn, excluding external package projects.
todos:
  - id: audit-layout
    content: Inventory current files/resources and map to target folders
    status: pending
  - id: create-skeleton
    content: Create new src/tests/docs/tools folder skeleton
    status: pending
  - id: move-assets
    content: Move icons/sounds/config into Assets and update csproj
    status: pending
  - id: move-interop
    content: Move interop/infrastructure classes and update csproj
    status: pending
  - id: move-ui
    content: Move XAML Views and update csproj includes
    status: pending
  - id: move-services
    content: Move App/services/common helpers into new folders
    status: pending
  - id: align-tests-docs
    content: Place tests/docs/tools into new locations and update solution
    status: pending
  - id: cleanup-verify
    content: Remove legacy roots and ensure builds succeed
    status: pending
---

# File Structure Reorg Plan

## Target layout (to be created, even if empty)

- Solution root
- `src/SimRacingFFB/` (main WPF app)
- `Application/` (composition, startup, DI, app settings)
- `Domain/` (entities, value objects, domain services)
- `Infrastructure/` (IO, telemetry integrations, device SDK wrappers)
- `Services/` (app-level services, orchestrators)
- `Interop/` (P/Invoke, external SDK wrappers: Logitech, vJoy)
- `Simulators/` (simulator-specific implementations using interfaces)
  - `IRacing/` (iRacing-specific code, e.g., `App.IRacingSDK.cs`)
  - `AssettoCorsa/` (placeholder for future AC support)
  - `AssettoCorsaCompetizione/` (placeholder for future ACC support)
  - `AssettoCorsaEvo/` (placeholder for future AC Evo support)
  - `RFactor/` (placeholder for future RFactor support)
- `UI/`
  - `Views/` (XAML + code-behind)
  - `ViewModels/`
  - `Controls/`
  - `Resources/` (resource dictionaries)
- `Assets/` (icons, sounds, images; move png/ico/wav here)
- `Properties/` (keep auto-generated)
- `Configuration/` (App.config, JSON settings)
- `Extensions/` (extension methods like `SerializableDictionary` helpers)
- `Common/` (shared helpers, utilities)
- `tests/SimRacingFFB.Tests/` (existing test project; add stubs if missing)
- `docs/` (README, telemetry notes, style guides)
- `tools/` (installer scripts like `InstallScript.iss`)
- `build/` (CI/build scripts if added later)

## Incremental move plan (minimal code change per step)

1) **Inventory & map**: catalog current files (assets, XAML, services/helpers like `App.*.cs`, `LogitechGSDK.cs`, `SerializableDictionary.cs`, `SpeechApiReflectionHelper.cs`) and their resource usages in `SimRacingFFB.csproj`. Exclude `IRSDKSharper` and `SimagicHPR` since they will become external packages.
2) **Create folders**: add new directory skeleton under `src/` and `tests/` without moving files; no code changes yet.
3) **Phase 1 – assets/config**: move icons/wavs into `Assets/`; update `SimRacingFFB.csproj` `Resource` and `ApplicationIcon` paths only; build check; **run application check**.
4) **Phase 2 – infrastructure/interop**: move external SDK wrappers and interop helpers (`LogitechGSDK.cs`, `WinApi.cs`, `FFBReceiver.cs`, `DeviceChangeNotification.cs`, `LoopStream.cs`, `SpeechApiReflectionHelper.cs`) into `Interop/` or `Infrastructure/`; adjust csproj includes; build check; **run application check**.
5) **Phase 3 – UI**: move XAML windows (`MainWindow.xaml`, `MapButtonWindow.xaml`, etc.) into `UI/Views/`; keep code-behind paired; update csproj `Page` includes if needed; update `App.xaml` StartupUri path; build check; **run application check**.
6) **Phase 4 – app/services/common**: move `App.*.cs` partials into `Application/` or `Services/`; move utilities (`Serializer.cs`, `ClipboardHelper.cs`, `Settings.cs`) into `Common/` or `Services/`; update csproj; build check; **run application check**.
7) **Phase 5 – simulators**: move iRacing-specific code (e.g., `App.IRacingSDK.cs`) into `Simulators/IRacing/`; update csproj; build check; **run application check**.
8) **Phase 6 – tests/docs/tools**: ensure `tests/SimRacingFFB.Tests/` exists; place `README.md`/notes in `docs/`; move `InstallScript.iss` to `tools/`; update solution if paths change; build check; **run application check**.
9) **Cleanup**: remove empty legacy directories from root, verify resource links still resolve, and tidy solution folder mappings; **final build and run application check**.

## Notes

- `IRSDKSharper` and `SimagicHPR` are assumed to be consumed as NuGet-like packages, not part of the workspace; do not create folders for them.
- Keep moves small per phase and run a build after each to catch broken resource includes or relative paths.
- **After each successful build, run the application to verify it starts correctly and resources load properly** (checkpoint verification).
- Avoid code edits unless a path change requires a project include update.
- Add future `ViewModels/Interfaces` folders now to encourage MVVM separation later.
- The `Simulators/` directory structure supports future multi-simulator support using interfaces; currently only iRacing implementation is needed.

