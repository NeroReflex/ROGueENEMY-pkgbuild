pkgname=rogue-enemy
pkgver=2.1.1
pkgrel=1
pkgdesc='Convert ROG Ally [RC71L] input to DualShock4 or DualSense and allows mode switching and easy thermal profile change'
arch=('x86_64')
url='https://github.com/NeroReflex/ROGueENEMY/'
license=('GPLv2')
depends=(
    'libconfig'
    'libevdev'
)
makedepends=('cmake')
provides=('rogue-enemy')
source=(
    "https://github.com/NeroReflex/ROGueENEMY/archive/refs/tags/v2.1.1.tar.gz"
    "rogue-enemy.service"
    "stray-ally.service"
)
sha256sums=(
    '94c28d03c9e0ed69f49e42a5f06211c194c9f34bf358137d918a025cbf3e046a' # source code
    'a9488856982a2ecea04c65db7ea231bbf7d656a60754c83e2a6365f86349b642' # rogue-enemy.service
    'ab7329876393bc4f28f578e377e4c0443a7af82db7a0d20cb2ec51e403276e6e' # stray-ally.service
)
options=(lto)

prepare() {
    cd "ROGueENEMY-$pkgver"

    #mkdir "ally-motion-evdev/build"
    cd ..
}

build() {
    CFLAGS+=" -ffat-lto-objects"
    cmake \
        -B build \
        -S "ROGueENEMY-$pkgver" \
        -G 'Unix Makefiles' \
        -DCMAKE_BUILD_TYPE:STRING='Release' \
        -DCMAKE_INSTALL_PREFIX:PATH='/usr' \
        -Wno-dev

    cmake --build build
}

#check() {
#    ctest --test-dir build --output-on-failure
#}

package() {
    DESTDIR="$pkgdir" cmake --install build

    # systemd
    install -D -m644 rogue-enemy.service   -t "${pkgdir}/usr/lib/systemd/system/"
    install -D -m644 stray-ally.service   -t "${pkgdir}/usr/lib/systemd/system/"

    mkdir -p "$pkgdir/etc/ROGueENEMY"

    install -D -m755 "ROGueENEMY-$pkgver/rogue-enemy_iio_buffer_on.sh" -t "$pkgdir/usr/bin/"
    install -D -m755 "ROGueENEMY-$pkgver/rogue-enemy_iio_buffer_off.sh" -t "$pkgdir/usr/bin/"
    install -D -m644 "ROGueENEMY-$pkgver/80-playstation.rules" -t "$pkgdir/usr/lib/udev/rules.d/"
    install -D -m644 "ROGueENEMY-$pkgver/80-playstation-no-libinput.rules" -t "$pkgdir/usr/lib/udev/rules.d/"
    install -D -m644 "ROGueENEMY-$pkgver/99-js-block.rules" -t "$pkgdir/usr/lib/udev/rules.d/"
    install -D -m644 "ROGueENEMY-$pkgver/99-xbox360-block.rules" -t "$pkgdir/usr/lib/udev/rules.d/"

    install -D -m644 "ROGueENEMY-$pkgver/config.cfg.default" -t "$pkgdir/etc/ROGueENEMY/"
    mv "$pkgdir/etc/ROGueENEMY/config.cfg.default" "$pkgdir/etc/ROGueENEMY/config.cfg"

    # license
    install -D -m644 "ROGueENEMY-$pkgver/LICENSE.md" -t "${pkgdir}/usr/share/licenses/${pkgname}"
}