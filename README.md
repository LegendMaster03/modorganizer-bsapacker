# BSA Packer — Mod Organizer 2

BSA Packer is a Mod Organizer 2 plugin for packaging loose mod assets into archives supported by the active game. The plugin metadata credits **MattyFez & MO2 Team**; this repository includes integration with the shared `MOBase::GameArchiveHandler` game-feature API.

## Archive creation paths

- **Built-in archives:** When the active game exposes `MOBase::DataArchives`, the plugin uses its archive builders and creates the appropriate `.bsa` or `.ba2` output. This path can also create companion plugin/override files.
- **Game-provided formats:** If the game does not expose the built-in tools but does provide `GameArchiveHandler`, the plugin delegates creation to the active game's `createArchive` implementation. The format and output depend on that game plugin.
- **Unsupported games:** Without either capability, the plugin reports that archive packing is unsupported.

The paths are intentionally distinct. A game with both capabilities uses the built-in path; custom-handler archive creation does not automatically create the Bethesda-specific override or dummy-plugin files.

## File effects and failure behavior

Archive packing modifies the selected mod directory. The custom-handler path currently **deletes an existing destination archive before attempting replacement**; if creation fails, the previous archive is not automatically restored. Successful packing can hide loose assets. Review and back up mod content before using the plugin on important files.

## Dependencies and testing

Build against a matching Mod Organizer 2 `uibase` providing `DataArchives` and `GameArchiveHandler`, plus the project's Qt and archive-builder dependencies. The original development environment used Qt 5.12.5 and QtConcurrent; that historical note is not a guarantee of compatibility with all current MO2 releases.

The `tests/` directory contains coverage of the archive builder, naming services, and worker behavior. The plugin's available formats depend on the active game's registered features.
