# Sources and verification

- Fabric 26.3 release/toolchain information: https://fabricmc.net/2026/09/15/263.html
- Fabric development page: https://www.fabricmc.net/develop/
- Fabric key mappings documentation: https://docs.fabricmc.net/develop/key-mappings

## Version 0.9.0 verification
- Parsed the JSON metadata/config/language files with Python's JSON parser.
- Checked Java source brace balance and ZIP integrity.
- Did **not** compile, launch, or test the mod in Minecraft. The environment has Java 21 only and no Gradle executable; required build tools and dependencies could not be fetched.
