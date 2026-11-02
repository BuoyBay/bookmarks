# Create and resolve macOS file bookmarks

Persist native NSURL bookmark bytes instead of assuming a saved path stays valid.
`Import bookmarks`. macOS 11.2+ is required by the resolution options used here.

Start with the [installation and complete usage example](resources/buoy-docs/getting-started.md), including explicit close and sandbox requirements. The [API reference](resources/buoy-docs/api.md) describes errors and access ownership.

## Install

From a Buoy consumer project with BPM installed:

```sh
bpm add bookmarks --git https://github.com/BuoyBay/bookmarks.git --rev refs/heads/main
```

BPM records the resolved commit in its lockfile. Keep that lockfile with the
consumer project for reproducible installs.

## Example

```basic
Import bookmarks

Sub InspectChosenFile(file As FileSystemItem)
    If Not BookmarksSupported() Then Return
    Dim data As Bytes = CreateBookmark(file)
    Dim resolved As New ResolvedBookmark(data)
    Try
        Print resolved.File.FullPath()
        If resolved.IsStale Then data = resolved.Refresh(True)
    Finally
        resolved.Close()
    End Try
End Sub
```

## Platform and scope

Native bookmark operations require macOS. Keep security-scoped access ownership explicit; unsupported targets must be checked before use.

## Build from source

With the native `build-packages` driver from [devtools](https://github.com/BuoyBay/devtools)
on your `PATH`, run from this checkout:

```sh
build-packages --root "$PWD" --compile-consumer bookmarks
```

The driver checks source targets and compiles an import consumer. See the
[API guide](docs/API.md) for behavior, limits and any host prerequisites.

## Local Buoy setup

On your own machine, install Buoy and run:

```sh
buoy ai init --both
```


## Contributing

[Contributing](CONTRIBUTING.md) · [License](LICENSE)
