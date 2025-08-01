```
Commands:
  electron-builder build                    Build                      [default]
  electron-builder install-app-deps         Install app deps
  electron-builder node-gyp-rebuild         Rebuild own native code
  electron-builder create-self-signed-cert  Create self-signed code signing cert
                                            for Windows apps
  electron-builder start                    Run application in a development
                                            mode using electron-webpack
  electron-builder publish                  Publish any files with your publish config

Building:
  --mac, -m, -o, --macos   Build for macOS, accepts target list (see
                           https://www.electron.build/mac)               [array]
  --linux, -l              Build for Linux, accepts target list (see
                           https://www.electron.build/linux)             [array]
  --win, -w, --windows     Build for Windows, accepts target list (see
                           https://www.electron.build/win)               [array]
  --x64                    Build for x64                               [boolean]
  --ia32                   Build for ia32                              [boolean]
  --armv7l                 Build for armv7l                            [boolean]
  --arm64                  Build for arm64                             [boolean]
  --universal              Build for universal (mac only)              [boolean]
  --dir                    Build unpacked dir. Useful to test.         [boolean]
  --prepackaged, --pd      The path to prepackaged app (to pack in a
                           distributable format)
  --projectDir, --project  The path to project directory. Defaults to current
                           working directory.
  --config, -c             The path to an electron-builder config. Defaults to
                           `electron-builder.yml` (or `json`, or `json5`, or `js`, or `ts`), see
                           https://goo.gl/YFRJOM

Publishing:
  --publish, -p  Publish artifacts (to GitHub Releases), see
                 https://www.electron.build/publish
                [choices: "onTag", "onTagOrDraft", "always", "never", undefined]


-----

electron-builder publish

Publish a list of artifacts

Options:
  -v, --version  The app/build version used when searching for an upload release
                 (used by some Publishers)                              [string]
      --help     Show help                                             [boolean]
  -f, --files    The file(s) to upload to your publisher      [array] [required]
  -c, --config   The path to an electron-builder config. Defaults to
                 `electron-builder.yml` (or `json`, or `json5`, or `js`, or
                 `ts`), see https://www.electron.build/configuration   [string]

Other:
  --help     Show help                                                 [boolean]
  --version  Show version number                                       [boolean]
```

For other commands please see help using `--help` arg, e.g. `./node_modules/.bin/electron-builder install-app-deps --help`

!!! tip
    Since Node.js 8 [npx](https://medium.com/@maybekatz/introducing-npx-an-npm-package-runner-55f7d4bd282b) is bundled, so, you can simply use `npx electron-builder`.


Prepend `npx` to sample commands below if you run it from Terminal and not from `package.json` scripts.

!!! example "build for macOS, Windows and Linux"
    `electron-builder -mwl`

!!! example "build deb and tar.xz for Linux"
    `electron-builder --linux deb tar.xz`

!!! example "build NSIS 32-bit installer for Windows"
    `electron-builder --windows nsis:ia32`

!!! example "set package.json property `foo` to `bar`"
    `electron-builder -c.extraMetadata.foo=bar`

!!! example "configure unicode options for NSIS"
    `electron-builder -c.nsis.unicode=false`

## Target

Without target configuration, electron-builder builds Electron app for current platform and current architecture using default target.

* macOS - DMG and ZIP for Squirrel.Mac.
* Windows - [NSIS]./nsis.md).
* Linux:
    - if you build on Windows or macOS: [Snap](./snap.md) and [AppImage](./appimage.md) for x64.
    - if you build on Linux: [Snap](./snap.md) and [AppImage](./appimage.md) for current architecture.

Platforms and archs can be configured or using [CLI args](https://github.com/electron-userland/electron-builder#cli-usage), or in the configuration.

For example, if you don't want to pass `--ia32` and `--x64` flags each time, but instead build by default NSIS target for all archs for Windows:

!!! example "Configuration"

    package.json
    ```json
    "build": {
      "win": {
        "target": [
          {
            "target": "nsis",
            "arch": [
              "x64",
              "ia32"
            ]
          }
        ]
      },
      "mac": {
        "target": [
          {
            "target": "dmg",
            "arch": [
              "universal"
            ]
          }
        ]
      }
    }
    ```

    electron-builder.yml
    ``` yaml
     win:
       target:
         - target: nsis
           arch:
             - x64
             - ia32
    mac:
      target:
        - target: dmg
          arch: universal
    ```

    electron-builder.config.js
    ```js
    module.exports = {
      "win": {
        "target": [
          {
            "target": "nsis",
            "arch": [
              "x64",
              "ia32"
            ]
          }
        ]
      },
      "mac": {
        "target": [
          {
            "target": "dmg",
            "arch": [
              "universal"
            ]
          }
        ]
      }
    }
    ```

and use
```
build -wl
```

### TargetConfiguration
* **<code id="TargetConfiguration-target">target</code>** String - The target name. e.g. `snap`.
* <code id="TargetConfiguration-arch">arch</code> "x64" | "ia32" | "armv7l" | "arm64" | "universal" - The arch or list of archs.
