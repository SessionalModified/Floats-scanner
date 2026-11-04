# Floats

**Minecraft Java screenshare scanner for Windows.**

Floats inspects the running game's mods, selected process evidence, public account records, and retained filesystem activity. It brings the findings together in a searchable HTML report saved on the scanned computer.

## What can Floats detect?

### Client structure

The current structural catalogue covers these **13 client families**:

| | | |
| --- | --- | --- |
| Meteor | Wurst | LiquidBounce |
| SalHack | GameSense | BleachHack |
| Lambda | KAMI Blue | ForgeHax |
| Raven B+/B++ derivatives | FDP | TrollHack |
| Seppuku | | |

These rules look for specific class definitions and supported metadata. A client name or shared namespace alone is insufficient for a structural detection. Coverage applies to the included signatures, not every version or derivative of a client.

### Module categories

Floats checks module-style class names and string vocabulary across seven categories:

| Category | Examples |
| --- | --- |
| Combat | Kill Aura, Aim Assist, Silent Aim, TriggerBot, Shield Disabler |
| Crystal / Anchor PvP | Auto Crystal, Crystal Optimizer, Double Anchor, Anchor Exploder |
| Totem | Auto Totem, Hover Totem, Force Totem, Auto Retotem |
| Movement | Fast Bridge, Bridge Assist, Fast Place, Elytra Glide, InventoryMove |
| Utility / Automation | Auto Clicker, Auto Pot, Auto Armor, Chest Stealer, Bed Macro |
| ESP / Vision | Player ESP, Storage ESP, Entity ESP, X-Ray, Netherite Finder |
| Evasion / Anticheat bypass | Fake Lag, Ping Spoof, Self Destruct, String Cleaner, USN Journal Cleaner |

Module-name matches are **contextual findings**. They do not establish that a feature was enabled or used.

### Mod and runtime checks

- Active `mods` directories and supported loader overrides.
- JVM-supplied extra mod paths, including `-Dfabric.addMods`, Java argument files, and supported mod-list files.
- Selected mod/agent containers on classpaths and module paths.
- Fabric, Quilt, Forge, and NeoForge metadata; Java agent declarations; bounded nested JARs.
- Target/attack reference co-occurrence, self-location/deletion references, and instrumentation/attach references for manual review.
- Read-only, bounded Minecraft process-memory checks. Built-in memory findings require at least two distinct client-specific markers.
- Native-module and relevant Windows event/debugger configuration context.

### File changes and system activity

- JAR modification timestamps after an observed Minecraft Java launch.
- A bounded, read-only NTFS USN V2 journal tail for supported active mod-directory changes.
- Retained `.pf` deletion records mapped to the Windows Prefetch directory.
- Relevant event-log clear and system clock-change metadata.
- Available PowerShell 4104 records classified for targeted log/journal clearing, active Minecraft artifact changes, and Java process-manipulation terms.
- Active-instance Java connection/disconnection log context.

**A deleted Prefetch record does not prove its executable was deleted.** Updates, cleanup, administration, and editable local logs can explain findings. Missing retained records cannot exclude earlier changes.

## Getting started

1. Use a provided Floats build, or build from the source project when available.
2. Keep the editable `detections` folder beside `Floats.exe` if you want to customize rules. Embedded defaults are available if that folder is absent.
3. Start Minecraft Java Edition, then open **Floats.exe**.
4. Click **Scan**, wait for completion, and select **Show Results**.
5. Review findings alongside **Scan coverage** and **Mod inventory**. Use **Scan Again** to create a separate report.

The standard local build is `dist/Floats.exe`. A separate .NET runtime installation is unnecessary for the self-contained build. Administrator access may be needed for process memory, protected event logs, or the USN journal. Floats records unavailable checks and does not elevate automatically.

Reports are saved to:

```text
%LOCALAPPDATA%\Floats\Reports\<timestamp>-<scan-id>\
  results.html
  results.json
```

## Reading the report

| Category | Contents |
| --- | --- |
| Detections | Specific structural client matches |
| Warnings | Heuristics and uncertain evidence requiring review |
| Suspicious Strings | Contextual module names, imported literals, and qualifying memory markers |
| Minecraft Accounts | Supported public Minecraft identity records |
| Discord Accounts | Supported local RPC identity or user-provided public records |
| File Changes | Supported JAR changes and retained Prefetch deletion records |

## Security and privacy

- Process memory is read-only; mod code is inspected without execution.
- No Discord tokens, messages, stored sessions, or credential stores are scanned.
- Reports remain local. Public names are visible; UUIDs and public IDs can be masked in the interface, but remain in report data.
- **Modrinth recognition is online by default:** selected JAR SHA-512 hashes are sent to its API. File contents, identities, logs, and reports are not uploaded. CLI `--offline` disables this lookup.
- Opening an HTML report makes no remote requests. Background animations respect the browser's reduced-motion preference.
- Floats does not change process memory, system security settings, or the USN journal, and does not download or launch JournalTrace or other scanner binaries.

Use Floats on computers you own or have permission to inspect. Review reports before sharing: they contain public identities, local paths, and evidence metadata.
