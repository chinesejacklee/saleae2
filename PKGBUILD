# Maintainer: tugyan <bilaltasdelen at windowslive dot com>
# Contributor: jkdhn <aur@jkdhn.me>

pkgname=saleae-logic2
pkgver=2.4.14
pkgrel=1
pkgdesc="Debug hardware like a pro"
arch=("x86_64")
url="https://discuss.saleae.com/c/logic-2-0-software/7"
license=("unknown")
depends=(
	"libxcrypt-compat"
)
source=("https://downloads.saleae.com/logic2/Logic-${pkgver}-linux-x64.AppImage")
conflicts=("saleae-logic" "saleae-logic-beta")
provides=("saleae-logic" "saleae-logic-beta")
sha256sums=('aa215313cfc09f4ba6fde3fea30b44f1f7025f41279d3834d12d87e8e04edfc8')

build() {
	_file="Logic-${pkgver}-linux-x64.AppImage"
	chmod u+x "${_file}"
	"./${_file}" --appimage-extract

	_desktop="${srcdir}/squashfs-root/Logic.desktop"
	sed -i "/^Exec=/cExec=${pkgname}" "${_desktop}"
	sed -i "/^X-AppImage/d" "${_desktop}"
	sed -i "s/^X-AppImage-Version=/Version=/" "${_desktop}"
}

package() {
	mkdir "${pkgdir}/opt/"
	mv "${srcdir}/squashfs-root/usr" "${pkgdir}/usr"
	mv "${srcdir}/squashfs-root" "${pkgdir}/opt/${pkgname}"

	mkdir -p "${pkgdir}/usr/share/applications"
	mv "${pkgdir}/opt/${pkgname}/Logic.desktop" "${pkgdir}/usr/share/applications/${pkgname}.desktop"
	rm "${pkgdir}/opt/${pkgname}/Logic.png"
	rm "${pkgdir}/opt/${pkgname}/.DirIcon"
	rm "${pkgdir}/opt/${pkgname}/AppRun"
	rm "${pkgdir}/opt/${pkgname}/version"
	rm -rf "${pkgdir}/usr/lib/"

	install -Dm644 "${pkgdir}/opt/${pkgname}/resources/linux-x64/99-SaleaeLogic.rules" "${pkgdir}/etc/udev/rules.d/99-SaleaeLogic.rules"

	# Fix permissions (example: 700->755, 640->644)
	find "${pkgdir}"   -perm "/111" -exec chmod 755 \{\} \;
	find "${pkgdir}" ! -perm "/111" -exec chmod 644 \{\} \;
	chmod 4755 "${pkgdir}/opt/${pkgname}/chrome-sandbox"

	mkdir -p "${pkgdir}/usr/bin/"
	ln -s "/opt/${pkgname}/Logic" "${pkgdir}/usr/bin/${pkgname}"
}
