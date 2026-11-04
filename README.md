# Floats

**Minecraft Java screenshare scanner for Windows.**

Floats inspects the running game's mods, selected process evidence, public account records, and retained filesystem activity. It brings the findings together in a searchable HTML report saved on the scanned computer.

**Current build:** 0.5.1 · **Platform:** Windows x64 · **Interface:** C# / WPF

## Features

- **One-click scanning** — automatically identifies supported running Minecraft instances and their active mod paths.
- **Mod analysis** — inspects JAR metadata, class definitions, references, constants, and supported nested archives without running mod code.
- **Runtime context** — checks selected Java processes for specific memory markers, Java agents, and native-module context.
- **File Changes** — records supported JAR timeline changes and retained Prefetch deletion records.
- **Public account lists** — collects supported Minecraft names/UUIDs and offers a configurable Discord local-identity check.
- **Editable rules** — separate folders for client signatures, module categories, and imported string packs.
- **Local reports** — searchable HTML and companion JSON, with an activity browser, mod inventory, coverage notes, and evidence details.
- **Animated interface** — rounded window, orbital stars and auroras, a rotating gear loader, and a centered progress percentage.

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

**WI Zoom:** shared Wurst zoom code is treated as a warning when the full-client structural criteria are absent.

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

**Verified-source mods receive the same code checks.** A Modrinth hash match identifies a published source; it does not establish that a mod is safe or permitted by a server.

### Imported string packs

The default catalogue contains **404 rule folders**, including a personal TXT selection and JSON strings grouped into **382 client-labelled packs**. These imported labels have not been independently validated. They do not expand the 13-family structural detection claim.

Imported literal matches appear under **Suspicious Strings** and add **zero evidence-index points**. Short/common terms are excluded from imported RAM findings; those findings require two qualified namespace/path markers from the same pack. `KeyboardMixin` is excluded from the supplied selection.

### File changes and system activity

- JAR modification timestamps after an observed Minecraft Java launch.
- A bounded, read-only NTFS USN V2 journal tail for supported active mod-directory changes.
- Retained `.pf` deletion records mapped to the Windows Prefetch directory.
- Relevant event-log clear and system clock-change metadata.
- Available PowerShell 4104 records classified for targeted log/journal clearing, active Minecraft artifact changes, and Java process-manipulation terms.
- Active-instance Java connection/disconnection log context.

**A deleted Prefetch record does not prove its executable was deleted.** Updates, cleanup, administration, and editable local logs can explain findings. Missing retained records cannot exclude earlier changes.

## Account support

**Minecraft:** public names/UUIDs from supported game arguments, launcher profiles, identified portable launchers, supported In-Game Account Switcher v1 containers, and recent active-instance `.log` / `.log.gz` login observations. Account panels display deduplicated identities rather than individual log entries. Authentication payloads are skipped; IAS credentials are not decrypted.

**Discord:** an optional local RPC READY handshake can report the public identity of a responding running client. It needs your registered public Discord application ID. Historical alt enumeration, token-store scanning, and credential validation are not supported.

Configure Discord using either:

```powershell
./dist/Floats.exe --scan --discord-app-id YOUR_PUBLIC_APPLICATION_ID
```

Or save this to `%LOCALAPPDATA%\Floats\discord-app.json`:

```json
{"applicationId":"YOUR_PUBLIC_APPLICATION_ID"}
```

No client secret or token is required. Public records do not prove account ownership or ban evasion.

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

The theme follows a heuristic **evidence index**:

- **0–30:** green
- **31–70:** yellow
- **71–100:** red

The index is not a probability that someone is cheating. A zero score does not establish a clean computer. The report explains which evidence contributes; account counts, imported strings, file changes, and unavailable checks do not increase it.

## Security and privacy

- Process memory is read-only; mod code is inspected without execution.
- No Discord tokens, messages, stored sessions, or credential stores are scanned.
- Reports remain local. Public names are visible; UUIDs and public IDs can be masked in the interface, but remain in report data.
- **Modrinth recognition is online by default:** selected JAR SHA-512 hashes are sent to its API. File contents, identities, logs, and reports are not uploaded. CLI `--offline` disables this lookup.
- Opening an HTML report makes no remote requests. Background animations respect the browser's reduced-motion preference.
- Floats does not change process memory, system security settings, or the USN journal, and does not download or launch JournalTrace or other scanner binaries.

Use Floats on computers you own or have permission to inspect. Review reports before sharing: they contain public identities, local paths, and evidence metadata.

## Updating detections

Each client family, module category, or imported string pack has its own `rule.json` folder:

```text
detections/
  clients/meteor/rule.json
  clients/wurst/rule.json
  modules/1-combat/rule.json
  custom/personal/rule.json
  custom/<client-pack>/rule.json
```

Edit the distributed files and restart Floats. Set `"enabled": false` to disable a pack. Deleting an external file restores its embedded default. Invalid overrides retain the embedded version where present and produce a coverage warning.

For permanent source changes, edit `src/Floats.Core/Detections` and rebuild. Building replaces distributed rule files with the source versions. Adding an imported string does not create a confirmed structural detection.

## Building and testing

The source project requires Windows x64 and the **.NET 10 SDK**. The Java fixture tests also require a JDK with `javac --release 8` support.

```powershell
./build.ps1 -Test
./tests/packaged-smoke.ps1
./tests/active-packaged-smoke.ps1
```

`build.ps1` publishes the newest self-contained executable to **`dist/Floats.exe`**, replacing the previous build. With Playwright and Edge available, `node tests/report.test.cjs` checks the HTML report.

Version 0.5.1 passed **80 engine fixture checks**, rendered WPF checks, browser report checks, and packaged worker/controlled-JVM smoke tests. These validate the tested mechanisms; they are not accuracy measurements against a broad real-cheat corpus. Elevated live-volume USN access and a live configured Discord RPC connection have not been validated by those fixtures.

## Limitations

Floats is experimental. It cannot guarantee detection of every client, injection, self-destruct routine, or bypass. Private/obfuscated clients, erased history, unsupported launchers, and manually mapped native code can evade the available checks.

Full deleted-file reconstruction, full Prefetch execution-history parsing, complete injection provenance, and automatic rule updates are not implemented. Findings establish observations within the completed scan scope; they are not an automatic ban verdict. The current executable is unsigned.

## Credits

Research references include [MeowClientFucker](https://github.com/MeowTonynoh/MeowClientFucker), [MeowResolver](https://github.com/MeowTonynoh/MeowResolver), the [triggerbot mod analyzer](https://github.com/nbme-ss/powershell-scripts), and the [RedLotus account-checker description](https://itzicehere.gitbook.io/redlotusguide/screenshare-tools/redlotus-tools/redlotus-alt-checker). Their scanner code was not copied or executed by Floats.

Client anchor provenance is recorded in the source rule files. Third-party fonts and runtime components retain their own licenses. The repository license is in [LICENSE](LICENSE).
