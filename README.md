[![Donate](https://img.shields.io/badge/-%E2%99%A5%20Donate-%23ff69b4)](https://hmlendea.go.ro/fund.html)
[![License: GPL v3](https://img.shields.io/badge/License-GPLv3-blue.svg)](https://gnu.org/licenses/gpl-3.0)

# .NET Release Scripts

Small Bash scripts for building self-contained release packages for .NET apps across multiple runtimes.

## What this repository provides

The `dotnet/` folder contains one script per target .NET major version:

- `2.2.sh`
- `3.1.sh`
- `5.0.sh`
- `6.0.sh`
- `7.0.sh`
- `8.0.sh`
- `9.0.sh`
- `10.0.sh`

Each script:

1. runs `dotnet publish` for a predefined set of runtime identifiers (RIDs)
2. outputs the artifacts in `bin/Release`
3. cleans the temporary publish output after packaging

## Prerequisites

- Bash
- .NET SDK matching the script family you are using
- `zip` (used when the script packages output folders)
- `git` (used to infer app/repository name)

## Usage

Run the scripts from your app folder (project root or solution root):

```bash
bash /path/to/dotnet-release-scripts/dotnet/<major>.sh <version>
```

Example:

```bash
bash ./dotnet/8.0.sh 1.4.0
```

The `<version>` argument is required.

For the `.NET 7` script only, you may disable trimming:

```bash
bash ./dotnet/7.0.sh 1.4.0 --no-trim
```

(`--no-trimming` is also accepted.)

## Runtime matrix

The scripts publish for the following RIDs:

| Script | Runtime identifiers |
| --- | --- |
| `2.2.sh`, `3.1.sh`, `5.0.sh` | `linux-arm`, `linux-arm64`, `linux-x64`, `osx-x64`, `win-arm64`, `win-x64` |
| `6.0.sh`, `7.0.sh` | `linux-arm`, `linux-arm64`, `linux-x64`, `osx-arm64`, `osx-x64`, `win-arm64`, `win-x64` |
| `8.0.sh`, `9.0.sh` | `linux-arm64`, `linux-x64`, `osx-arm64`, `osx-x64`, `win-arm64`, `win-x64` |
| `10.0.sh` | `linux-arm`, `linux-arm64`, `linux-x64`, `osx-arm64`, `osx-x64`, `win-arm64`, `win-x64` |

## Output and naming

- Artifacts are written to your app's `bin/Release` directory.
- Temporary staging is done in `bin/Release/.publish-script-output` and removed at the end.
- Output file names are based on repository name, version, and architecture.

Packaging behavior differs by script generation:

- `2.2.sh` to `6.0.sh`: always creates `.zip` files.
- `7.0.sh` to `10.0.sh`: if the publish output contains one non-PDB file, it copies/renames that file directly; otherwise it creates a `.zip`.

## Project layout expectations

The scripts try to detect a main project automatically:

- if an `.sln` exists in the current directory, they pick the first project listed in it
- otherwise they use the current directory as the main project directory

Then they publish the first `.csproj` found in that directory.

## Known limitations

- Solution/project autodetection is heuristic-based (first `.sln`, first listed project, first `.csproj`).
- Some scripts rely on `RootNamespace` for binary renaming; unusual project files may produce unexpected names.
- Trimming options vary by script generation and .NET version.

## Contributing

Contributions are welcome.

Please:

- keep pull requests focused and consistent with existing style
- update documentation when behaviour changes

## License

Licensed under the GNU General Public License v3.0 or later.
See [LICENSE](./LICENSE) for details.