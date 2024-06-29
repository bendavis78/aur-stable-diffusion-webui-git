# Maintainer: Ben Davis <bendavis78@gmail.com>

_appname="stable-diffusion-webui"
_apprefix="/opt"
_appdataprefix="/var/opt"

pkgname="${_appname}-git"
pkgver="1.9.4.r0.gfeee37d"
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
    "aba2e154eb8f47a2ba5ddca33acfbed07c57c1ea"
    "f5bde2b6efa29c72f1e391135d40f25967427ac1"
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
