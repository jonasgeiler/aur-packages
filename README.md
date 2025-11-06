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
- [supercronic](https://aur.archlinux.org/packages/supercronic/)
- [supercronic-bin](https://aur.archlinux.org/packages/supercronic-bin/)
- [supercronic-git](https://aur.archlinux.org/packages/supercronic-git/)

## Development Setup

Clone this repository:

```bash
git clone git@github.com:jonasgeiler/aur-packages.git
# or
git clone https://github.com/jonasgeiler/aur-packages.git
```

And then add remotes for all AUR packages:

```bash
git remote add yaak ssh://aur@aur.archlinux.org/yaak.git
git remote add yaak-appimage ssh://aur@aur.archlinux.org/yaak-appimage.git
git remote add yaak-bin ssh://aur@aur.archlinux.org/yaak-bin.git
git remote add yaak-git ssh://aur@aur.archlinux.org/yaak-git.git
git remote add supercronic ssh://aur@aur.archlinux.org/supercronic.git
git remote add supercronic-bin ssh://aur@aur.archlinux.org/supercronic-bin.git
git remote add supercronic-git ssh://aur@aur.archlinux.org/supercronic-git.git
```

Lastly, register the `.githooks` directory as a Git hooks directory:

```bash
git config core.hooksPath .githooks
````

The hooks contain some useful scripts that help with the development process,
like automatically updating and adding the `.SRCINFO` file on commit.

## Adding a package

All usages of Bash variables (`${var}`) in the following steps are
placeholders and should be replaced with the actual values.

1. Add a remote for the new package:
   ```bash
   git remote add ${package} ssh://aur@aur.archlinux.org/${package}.git
   ```
2. Add the subtree for the package:
   ```bash
   git subtree add --prefix ${package} ${package} master
   ```
3. Create the `${package}/PKGBUILD` file.
4. Continue from step 2 in the [Updating a package](#updating-a-package) section.

## Updating a package

All usages of Bash variables (`${var}`) in the following steps are
placeholders and should be replaced with the actual values.

1. Bump the `pkgver` to the new version and reset `pkgrel` to `1` in the
   `${package}/PKGBUILD` file. Also update the `pkgdesc` and other info if
   necessary.
   Make sure to read the release notes or changelog of the package to see if
   there are any breaking changes, new dependencies or anything else that needs
   to be changed.
   > TIP: Changes to GitHub Action release workflows are usually good sources
   > of information about new dependencies, commands and other build steps.
2. Update the checksums:
   ```bash
   updpkgsums ${package}/PKGBUILD
   ```
3. Check `PKGBUILD` for errors:
   ```bash
   namcap -i ${package}/PKGBUILD
   ```
4. Try building the package:
   ```bash
   makepkg --dir ${package} --force --cleanbuild
   ```
   > TIP: Sometimes a clean build can take a long time. If you're in a hurry,
   > you can rebuild from an already compiled package by removing the
   > `--cleanbuild` flag, but generally it is safer to build from scratch.

   Alternatively, use `extra-x86_64-build` from the `devtools` package to try 
   building in a clean chroot, which often reveals missing build dependencies:
   ```bash
   cd ${package}
   extra-x86_64-build -c
   cd ..
   ```
5. Check the package for errors:
   ```bash
   namcap -i ${package}/${package}-${version}-${arch}.pkg.tar.zst
   ```
6. (Optional) Install the package and test it out:
   ```bash
   makepkg --dir ${package} --install
   ```
7. Update the `${package}/.SRCINFO` file:
   ```bash
   makepkg --dir ${package} --printsrcinfo > ${package}/.SRCINFO
   ```
8. Stage the changes, commit and push them to this repository:
   ```bash
   git add PKGBUILD .SRCINFO
   git commit -m "feat(${package}): update to ${major}.${minor}.${patch}"
   git push
   ```
9. Push the changes to the AUR repository:
   ```bash
   git subtree push --prefix ${package} ${package} master
   ```

## Bash Snippets

A little collection of copy-paste snippets for various tasks. Run these from the
root of the repository.

### Pull new changes of all packages

```bash
git subtree pull --prefix yaak yaak master --message "Merge subtree 'yaak'"
git subtree pull --prefix yaak-appimage yaak-appimage master --message "Merge subtree 'yaak-appimage'"
git subtree pull --prefix yaak-bin yaak-bin master --message "Merge subtree 'yaak-bin'"
git subtree pull --prefix yaak-git yaak-git master --message "Merge subtree 'yaak-git'"
git subtree pull --prefix supercronic supercronic master --message "Merge subtree 'supercronic'"
git subtree pull --prefix supercronic-bin supercronic-bin master --message "Merge subtree 'supercronic-bin'"
git subtree pull --prefix supercronic-git supercronic-git master --message "Merge subtree 'supercronic-git'"
```

### Update checksums of all packages

```bash
updpkgsums yaak/PKGBUILD
updpkgsums yaak-appimage/PKGBUILD
updpkgsums yaak-bin/PKGBUILD
updpkgsums yaak-git/PKGBUILD
updpkgsums supercronic/PKGBUILD
updpkgsums supercronic-bin/PKGBUILD
updpkgsums supercronic-git/PKGBUILD
```

### Check `PKGBUILD` of all packages

```bash
namcap -i yaak/PKGBUILD
namcap -i yaak-appimage/PKGBUILD
namcap -i yaak-bin/PKGBUILD
namcap -i yaak-git/PKGBUILD
namcap -i supercronic/PKGBUILD
namcap -i supercronic-bin/PKGBUILD
namcap -i supercronic-git/PKGBUILD
```

### Build all packages

```bash
makepkg --dir yaak --force --cleanbuild
makepkg --dir yaak-appimage --force --cleanbuild
makepkg --dir yaak-bin --force --cleanbuild
makepkg --dir yaak-git --force --cleanbuild
makepkg --dir supercronic --force --cleanbuild
makepkg --dir supercronic-bin --force --cleanbuild
makepkg --dir supercronic-git --force --cleanbuild
```

### Update `.SRCINFO` of all packages

```bash
makepkg --dir yaak --printsrcinfo > yaak/.SRCINFO
makepkg --dir yaak-appimage --printsrcinfo > yaak-appimage/.SRCINFO
makepkg --dir yaak-bin --printsrcinfo > yaak-bin/.SRCINFO
makepkg --dir yaak-git --printsrcinfo > yaak-git/.SRCINFO
makepkg --dir supercronic --printsrcinfo > supercronic/.SRCINFO
makepkg --dir supercronic-bin --printsrcinfo > supercronic-bin/.SRCINFO
makepkg --dir supercronic-git --printsrcinfo > supercronic-git/.SRCINFO
```

### Push changes of all packages

```bash
git subtree push --prefix yaak yaak master
git subtree push --prefix yaak-appimage yaak-appimage master
git subtree push --prefix yaak-bin yaak-bin master
git subtree push --prefix yaak-git yaak-git master
git subtree push --prefix supercronic supercronic master
git subtree push --prefix supercronic-bin supercronic-bin master
git subtree push --prefix supercronic-git supercronic-git master
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

### PKGBUILDs of multi-arch Golang packages

- https://aur.archlinux.org/packages/traefik-bin
