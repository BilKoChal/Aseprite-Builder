# Aseprite Windows Builder

This repo contains a GitHub Actions workflow that builds **Aseprite for Windows x64** from the official source repo.

It will:

- Clone `aseprite/aseprite` at the version you choose (tag/branch/commit).
- Auto-detect the correct Skia branch from `INSTALL.md` and download a prebuilt Skia package.
- Build Aseprite with CMake + Ninja on `windows-latest`.
- Patch the version so the window title shows the selected version (e.g. `Aseprite v1.3.16` instead of `v1.x-dev`).
- Bundle required OpenSSL DLLs so the EXE runs on a clean Windows install.
- Upload a zip with `aseprite.exe` and other runtime files.

> Note: This repo doesn’t contain Aseprite source; it only automates cloning and building from the official upstream project.

---

## How to build your own Aseprite

1. Go to this repo’s **Actions** tab.
2. Select the workflow named `Build Aseprite (Windows x64)`.
3. Click **Run workflow** (top-right).
4. Fill in:
   - `aseprite_ref`: version to build, e.g. `v1.3.16`, `v1.3.17-beta2`, `main`.
   - `skia_release_tag`: usually leave empty (auto).
   - `skia_asset_name`: default should work; if a run fails it will print valid names.
   - `build_type`: usually `RelWithDebInfo` or `Release`.
5. Click **Run workflow**.

When it finishes:

1. Open the finished run in the **Actions** tab.
2. Scroll to **Artifacts**.
3. Download the zip named like `aseprite-windows-x64-v1.3.16.zip`.
4. Extract it and run `aseprite.exe` from the extracted `bin` folder.

---

## Forking and customizing

To have your own copy:

1. Click **Fork** on this repo and create a fork under your account.
2. Enable workflows in your fork (GitHub might ask the first time).
3. Run the same workflow from your fork’s **Actions** tab.
4. Optionally edit `.github/workflows/build-aseprite.yml` to change defaults or add extra steps (themes, plugins, signing, etc.).

---

## Troubleshooting (short)

- **Skia asset not found**  
  The log will list available Skia asset names for that release. Copy one of those names into the `skia_asset_name` input and rerun.

- **DLL missing (e.g. `libcrypto-3-x64.dll`)**  
  The workflow already copies OpenSSL DLLs next to `aseprite.exe`. If you still see errors, make sure you’re running directly from the extracted folder without removing any DLLs.
