pkgname=sfx-k8s
pkgver=1.0.0
pkgrel=1
pkgdesc="My k8s Setup"
arch=('x86_64' 'aarch64')
url="https://sfxworks.net"
license=('GPL')
depends=('kubelet' 'kubeadm' 'crio' 'crun' 'open-iscsi' 'nfs-utils' 'libcrossguid' 'nvme-cli')
install=$pkgname.install

package() {
    # Copy your custom files for /etc/containers/registries.conf and /etc/crio/crio.conf
    install -Dm644 "$srcdir/registries.conf" "$pkgdir/etc/containers/registries.conf"
    install -Dm644 "$srcdir/crio.conf" "$pkgdir/etc/crio/crio.conf"

    # Copy the custom kubelet.env file
    install -Dm644 "$srcdir/kubelet.env" "$pkgdir/etc/kubernetes/kubelet.env"
}