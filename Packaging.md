# AUR Packaging guide

1. Create an empty git repository.
2. Set the remote to `ssh://aur@aur.archlinux.org/YOUR_PACKAGE_NAME.git`
2. Read the [creating packages guide](https://wiki.archlinux.org/index.php/Creating_packages).
3. Create a `PKGBUILD`. You can find an example below.
4. Commit your changes, push
5. You should be good to go

Read the usefuls tools section below.
They should help you a lot.

## Useful tools

### Tools to ensure valid files

- `updpkgsums` automatically downloads all specified sources and updates the MD5 sums in your PKGBUILD
- `makepkg --printsrcinfo > .SRCINFO` creates the required `.SRCINFO` file from the PKGBUILD

`updpkgsums` has to be executed everytime a source changes.\
The `.SRCINFO` has to be updated every time the `PKGBUILD` is touched.

This alias is pretty nice:
```
alias mksrcinfo='updpkgsums && makepkg --printsrcinfo > .SRCINFO'
```

It's recommended to just run this alias every time you changed something.

### Tools to ensure correct packaging

`extra-x86_64-build` excutes `makepkg` in a new clean arch-chroot with an systemd-nspawn container.
This ensures, that the program properly builds on other clean systems.

When building AUR packages, there usually are a few warnings at the end of the output, e.g.:

```
==> Running checkpkg
error: target not found: pueue-bin
==> WARNING: Skipped checkpkg due to missing repo packages
```

These can be ignored, since this is a check for official packages.

If everything looks fine, your should be good go to.

## Useful tips

### Source prefixing

Include `$pkgver` in your sourc names.
You can see an example of this in the example below, e.g. `"$pkgname-$pkgver.tar.gz::https://github.com/Nukesor/pueue/archive/v${pkgver}.tar.gz"`. \
Without this installs might fail due to overwritten file warnings, depending on the package manager used by the user.

Including the `$pkgver` in the source names, guarantees that different versions won't conflict if they're located in the same path.

### PKGBUILD only changes

When updating your `PKGBUILD` without updating the `pkgver`, you have to increment the `pkgrel` version.
This version is used to track these exact changes.
You cannot push otherwise.

## Example config

```
# Maintainer: Arne Beer <privat@arne.beer>

pkgname=pueue
pkgver='0.10.2'
pkgrel=1
arch=('any')
pkgdesc='A command scheduler for shells'
license=('MIT')
makedepends=('cargo')
url='https://github.com/nukesor/pueue'
source=(
    "$pkgname-$pkgver.tar.gz::https://github.com/Nukesor/pueue/archive/v${pkgver}.tar.gz"
)
md5sums=('1c3d14ee3e1724efc7b4f9badf51168c')

build() {
    cd "$srcdir/$pkgname-$pkgver"
    cargo build --release --locked

    ./build_completions.sh
}

package() {
    cd "$srcdir/$pkgname-$pkgver"

    # Install binaries
    install -Dm755 "target/release/pueue" "$pkgdir/usr/bin/pueue"
    install -Dm755 "target/release/pueued" "$pkgdir/usr/bin/pueued"

    # Place systemd user service
    install -Dm644 "utils/pueued.service" "$pkgdir/usr/lib/systemd/user/pueued.service"

    # Install shell completions file
    install -Dm644 "utils/completions/_pueue" "$pkgdir/usr/share/zsh/site-functions/_pueue"
    install -Dm644 "utils/completions/pueue.bash" "$pkgdir/usr/share/bash-completion/completions/pueue.bash"
    install -Dm644 "utils/completions/pueue.fish" "$pkgdir/usr/share/fish/completions/pueue.fish"

    # Install License
    install -Dm644 "LICENSE" "$pkgdir/usr/share/licenses/pueue/LICENSE"
}
```
