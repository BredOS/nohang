# Maintainer: Bill Sideris <bill88t@feline.gr>
# shellcheck disable=SC2034,SC2148,SC2154,SC2164

pkgname=bredos-nohang
pkgver=0.2.0
pkgrel=3
pkgdesc="A sophisticated low memory handler, tailored for SBCs"
arch=('any')
url="https://github.com/hakavlad/nohang"
license=('MIT')
source=(
	"$pkgname::git+https://github.com/hakavlad/nohang.git#commit=bf477da78041a1fce8a82d04da422ef60e86c556"
	"patch.diff"
	"nohang-desktop.conf"
)
md5sums=(
    'SKIP'
    'SKIP'
    'SKIP'
)
depends=(
	'python'
	'systemd'
)
optdepends=(
	'libnotify: to show GUI notifications'
	'sudo: to show GUI notifications if nohang started with UID=0'
	'logrotate: maintaining a separate log is one of the options'
)
makedepends=(
	'git'
)
provides=('bredos-nohang')
conflicts=(
    'nohang-git'
    'nohang'
)
backup=(
	'etc/nohang/nohang.conf'
	'etc/nohang/nohang-desktop.conf'
	'etc/logrotate.d/nohang'
)

package() {
	cd "${srcdir}/${pkgname}" || exit 2
	git apply "${srcdir}/patch.diff"
	cp "${srcdir}/nohang-desktop.conf" "conf/nohang/nohang-desktop.conf.in"
	make \
		DESTDIR="${pkgdir}" \
		PREFIX="/usr" \
		SBINDIR="/usr/bin" \
		SYSCONFDIR="/etc" \
		SYSTEMDUNITDIR="/usr/lib/systemd/system" \
		install
	install -Dm644 LICENSE "$pkgdir/usr/share/licenses/$pkgname/LICENSE"
}

post_install() {
    systemctl enable --now nohang-desktop.service
}
post_upgrade() {
    post_install
}
post_remove() {
    systemctl disable --now nohang-desktop.service
}

