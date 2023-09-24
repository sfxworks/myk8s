pkgname=sfx-k8s
pkgver=1.0.0
pkgrel=1
pkgdesc="My k8s Setup"
arch=('x86_64 aarch64')
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

post_install() {
    # Load required modules
    modprobe uio
    modprobe uio_pci_generic
    modprobe nvme-tcp
    modprobe br_netfilter

    # Add modules to /etc/modules-load.d/sfx-k8s.conf to autoload at boot
    echo -e "uio\nuio_pci_generic\nnvme-tcp\nbr_netfilter" > /etc/modules-load.d/sfx-k8s.conf

    # Sysctl configurations
    echo "net.bridge.bridge-nf-call-ip6tables = 1" > /etc/sysctl.d/sfx-k8s.conf
    echo "net.bridge.bridge-nf-call-iptables = 1" >> /etc/sysctl.d/sfx-k8s.conf
    echo "vm.nr_hugepages=512" >> /etc/sysctl.d/sfx-k8s.conf

    # Reload sysctl to apply changes immediately
    sysctl --system

    # Reload daemons or restart services if needed
    systemctl restart crio.service kubelet.service
}

post_remove() {
    # Cleanup tasks when package is removed
    rm /etc/modules-load.d/sfx-k8s.conf
    rm /etc/sysctl.d/sfx-k8s.conf
}
