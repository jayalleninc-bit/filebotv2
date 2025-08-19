# GitHub MSI Build Setup

This folder contains everything needed for GitHub Actions to build an **MSI** for your app.

## What’s included
- `.github/workflows/main.yml` — CI that builds your .NET 8 WPF & CLI, harvests files with WiX Heat, and produces an MSI.
- `tools/wix/Product.wxs` — WiX definition with Start Menu shortcut.
- (Optional) set `WINDOWS_CERT_BASE64` and `WINDOWS_CERT_PASSWORD` as GitHub Secrets to sign binaries & MSI.

## Expected repo layout
```
src/
  MediaRanger.Wpf/MediaRanger.Wpf.csproj
  MediaRanger.Cli/MediaRanger.Cli.csproj
tools/
  wix/Product.wxs
.github/
  workflows/main.yml
```

If your paths differ, edit the env section in `main.yml`:
```
WPF_CSProj, CLI_CSProj, MAIN_EXE
```

## How it works
1. Builds both projects in Release.
2. Publishes to `out/wpf` and `out/cli`.
3. Uses **WiX Heat** to harvest all files into .wxs fragments.
4. Compiles `Product.wxs` + fragments and links them into `MediaRanger-<version>.msi`.
5. Uploads artifacts; on tags starting with `v`, creates a Release.
