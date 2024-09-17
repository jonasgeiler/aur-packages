# aur-packages

> My collection of packages I maintain for the Arch User Repository.

In this repository, I keep the `PKGBUILD` files and other necessary files for
the packages I maintain in the Arch User Repository (AUR). This repository is
used to keep track of changes to the packages and to make it easier to monitor
and update them all at once.

## Packages

- [yaak](https://aur.archlinux.org/packages/yaak/)
- [yaak-appimage](https://aur.archlinux.org/packages/yaak-appimage/)
- [yaak-bin](https://aur.archlinux.org/packages/yaak-bin/)
- [yaak-git](https://aur.archlinux.org/packages/yaak-git/)

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

### Update subtrees

```bash
git subtree pull --prefix yaak yaak master
git subtree pull --prefix yaak-appimage yaak-appimage master
git subtree pull --prefix yaak-bin yaak-bin master
git subtree pull --prefix yaak-git yaak-git master
```

## Updating a package

1. Bump the `pkgver` to the new version and reset `pkgrel` to `1` in the
   `PKGBUILD` file. Also update the `pkgdesc` and other info if necessary.
   Make sure to read the release notes or changelog of the package to see if
   there are any breaking changes, new dependencies or anything else that needs
   to be changed.
   > TIP: Changes to GitHub Action release workflows are usually good sources
   > of information about new dependencies, commands and other build steps.
2. Update the checksums:
   ```bash
   updpkgsums
   ```
3. Check `PKGBUILD` for errors:
   ```bash
   namcap -i PKGBUILD
   ```
4. Try building the package:
   ```bash
   makepkg --force --cleanbuild
   ```
   > TIP: Sometimes a clean build can take a long time. If you're in a hurry,
   > you can rebuild from an already compiled package by removing the
   > `--cleanbuild` flag, but generally it is safer to build from scratch.

   Alternatively, use `extra-x86_64-build` from the `devtools` package to try 
   building in a clean chroot, which often reveals missing build dependencies:
   ```bash
   extra-x86_64-build -c
   ```
5. Check the package for errors:
   ```bash
   namcap -i *.pkg.tar.zst
   ```
6. (Optional) Install the package and test it out:
   ```bash
   makepkg --install
   ```
7. Update the `.SRCINFO` file:
   ```bash
   makepkg --printsrcinfo > .SRCINFO
   ```
8. Stage the changes, commit and push them to this repository:
   ```bash
   git add PKGBUILD .SRCINFO
   git commit -m 'feat(<pkg>): updated to x.y.z'
   git push
   ```
9. Push the changes to the AUR repository:
   ```bash
   git subtree push --prefix yaak yaak master
   # or
   git subtree push --prefix yaak-appimage yaak-appimage master
   # or
   git subtree push --prefix yaak-bin yaak-bin master
   # or
   git subtree push --prefix yaak-git yaak-git master
   ```

## Reference Links

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
