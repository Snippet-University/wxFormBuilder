# Health Report — 1970ai Forms (wxFormBuilder)

## Overview

This document describes the current build and CI health of the **1970ai Forms** project
(formerly wxFormBuilder), identifies known errors, and records the last verified successful build.

---

## CI Build Status

| CI Provider | Status | Notes |
|-------------|--------|-------|
| AppVeyor (Windows) | ⚠️ Outdated | References MSYS2/MinGW32 stack; `create_build_files4.bat` relies on premake-generated Makefiles that require manual sed patching. |
| Travis CI (Linux/macOS) | ❌ Inactive | travis-ci.org was shut down for open-source projects. The `.travis.yml` config references the deprecated service endpoint. |

### Last Successful Build

| Field | Value |
|-------|-------|
| Branch | `master` |
| Commit SHA | `bef14969d5743bcdc9c27c9f763fa5cc5e004915` |
| Commit message | Merge pull request #547 — Added support for wxBitmapToggleButton |
| Date | 2019-11-01 |
| AppVeyor artifact | `wxFormBuilder_win32.zip` |

---

## Known Errors

### Build System

1. **Deprecated Travis CI endpoint** (`travis-ci.org`): The `.travis.yml` file targets
   `travis-ci.org`, which is no longer active for open-source repositories.
   *Recommendation*: Migrate to GitHub Actions.

2. **Outdated Fedora 28 Docker image**: The Linux flatpak build uses `fedora:28`, which
   is no longer receiving security updates.
   *Recommendation*: Update to a supported Fedora release.

3. **Brew tap dependency** (`cdalvaro/tap/wxmac`): The macOS build depends on a
   third-party Homebrew tap that may be unmaintained.
   *Recommendation*: Use the official `wxwidgets` Homebrew formula instead.

4. **Manual `sed` patching in AppVeyor**: The Windows build requires sed one-liners to
   fix linker flag ordering and `-lbfd` references. This is fragile.
   *Recommendation*: Switch to the meson build system on all platforms.

### Runtime / Code Generation

6. **Python indentation defaults to tabs**: The Python code generator defaults to
   tab-based indentation. [PEP 8](https://peps.python.org/pep-0008/#indentation)
   recommends 4 spaces.
   *Fix*: Set `indent_with_spaces` to `1` by default in `output/xml/default.xml`.
   *(Applied in this PR.)*

---

## Reverting to the Last Successful Build

The last verified build that produced working binaries is commit
`bef14969d5743bcdc9c27c9f763fa5cc5e004915` on the `master` branch.

To return to that state locally:

```sh
git checkout bef14969d5743bcdc9c27c9f763fa5cc5e004915
```

The corresponding Windows installer artifact is available at:

```
https://ci.appveyor.com/api/projects/jhasse/wxformbuilder-461d5/artifacts/wxFormBuilder_win32.zip?branch=master
```

---

## Recommended Next Steps

- [ ] Migrate CI from Travis CI + AppVeyor to **GitHub Actions**
- [ ] Update the Fedora Docker image to a current release
- [ ] Switch all platforms to the **meson** build system
- [ ] Replace the third-party `wxmac` Homebrew tap with the official `wxwidgets` formula
- [ ] Fix the duplicate copyright line in `codewriter.cpp`
