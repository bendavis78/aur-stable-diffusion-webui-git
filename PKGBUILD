# Maintainer: Ben Davis <bendavis78@gmail.com>

_appname="stable-diffusion-webui"
_apprefix="/opt"
_appdataprefix="/var/opt"

pkgname="${_appname}-git"
pkgrel=1
pkgdesc="Stable Diffusion Web UI (AUTOMATIC1111)"
arch=("x86_64")
url="https://github.com/AUTOMATIC1111/$_appname"
license=("AGPL3")
groups=()
depends=("python310" "wget")
makedepends=("git")
provides=("stable-diffusion-ui")
source=(
    "${pkgname}::git+https://github.com/AUTOMATIC1111/$_appname.git"
    "stable-diffusion-webui.service"
    "webui.conf"
)
install="${pkgname}.install"
noextract=("v1-5-pruned-emaonly.safetensors")
sha1sums=(
    "SKIP"
    "f6b4f95b9e580401acfc6151572d15edb990186e"
    "59c7d218baa0c1577c3f739f01910a1d90da2071"
)
options=('!strip')

pkgver() {
    cd "$srcdir/$pkgname"
    git describe --long --tags --abbrev=7 | sed 's/^v//;s/\([^-]*-g\)/r\1/;s/-/./g'
}

package() {
    # Install systemd service
    install -Dm644 "./$_appname.service" "$pkgdir/usr/lib/systemd/system/$_appname.service"
    rm -rf "$pkgdir/opt/$_appname/.git"

    # Install the default config file to /usr/share/$_appname/webui.conf
    install -d "$pkgdir/usr/share/$_appname"
    install -Dm644 "./webui.conf" "$pkgdir/usr/share/$_appname/webui.conf"

    # Copy source to app's home directory
    parent_dir="$pkgdir${_apprefix}"  # /opt

    install -d "$pkgdir${_apprefix}/$_appname"
    install -d "$pkgdir${_appdataprefix}/$_appname"

    cp -R "$srcdir/${pkgname}/." "$pkgdir${_apprefix}/$_appname"
    chmod 775 "$pkgdir${_apprefix}/$_appname"
    chmod -R gu+rwX,go+r "$pkgdir${_apprefix}/$_appname"
}
