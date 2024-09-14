# aur-packages

> My collection of packages I maintain for the Arch User Repository.

## Setup

### Clone the repository

```bash
git clone git@github.com:jonasgeiler/aur-packages.git
# or
git clone https://github.com/jonasgeiler/aur-packages.git
```

### Add remotes for the AUR packages

```bash
git remote add yaak ssh://aur@aur.archlinux.org/yaak.git
git remote add yaak-appimage ssh://aur@aur.archlinux.org/yaak-appimage.git
git remote add yaak-bin ssh://aur@aur.archlinux.org/yaak-bin.git
git remote add yaak-git ssh://aur@aur.archlinux.org/yaak-git.git
```

## Maintenance

### Build a package

```bash
makepkg --force --cleanbuild
```

### Check package sanity

```bash
namcap -i PKGBUILD
namcap -i *.pkg.tar.zst
```

Some errors/warnings can be ignored, like the "ELF File" and "License" warnings.

### Install a package

```bash
makepkg --install
```

### Updating a package

- Update the `pkgver` and `pkgrel` variables in the `PKGBUILD` file.
- Update `pkgdesc` or other info if necessary.
- Update the checksums:
  ```bash
  updpkgsums
  ```
- Update the `.SRCINFO` file:
  ```bash
  makepkg --printsrcinfo > .SRCINFO
  ```
- Commit the changes.

<!-- TODO: Add info about updating git subtrees or something -->

## Useful Links

### Popular PKGBUILDs

- https://aur.archlinux.org/cgit/aur.git/tree/PKGBUILD?h=yay
- https://aur.archlinux.org/cgit/aur.git/tree/PKGBUILD?h=yay-bin
- https://aur.archlinux.org/cgit/aur.git/tree/PKGBUILD?h=yay-git
- https://aur.archlinux.org/cgit/aur.git/tree/PKGBUILD?h=paru
- https://aur.archlinux.org/cgit/aur.git/tree/PKGBUILD?h=paru-bin
- https://aur.archlinux.org/cgit/aur.git/tree/PKGBUILD?h=paru-git

### PKGBUILD templates for AppImage packages

- https://daveparrish.net/posts/2019-11-16-Better-AppImage-PKGBUILD-template.html

### PKGBUILDs of AppImage packages

- https://aur.archlinux.org/cgit/aur.git/tree/PKGBUILD?h=joplin-appimage
- https://aur.archlinux.org/cgit/aur.git/tree/PKGBUILD?h=wootility-appimage
- https://aur.archlinux.org/cgit/aur.git/tree/PKGBUILD?h=freecad-appimage
- https://aur.archlinux.org/cgit/aur.git/tree/PKGBUILD?h=voicevox-appimage
- https://aur.archlinux.org/cgit/aur.git/tree/PKGBUILD?h=citra-appimage
