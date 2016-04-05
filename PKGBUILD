#Maintainer: Jason R. McNeil <jason@jasonrm.net>
#Contributor: Iwan Timmer <irtimmer@gmail.com>

pkgname=kubernetes-mesos-git
pkgver=r27242.b8d0008
pkgrel=1
pkgdesc="Container Cluster Manager for Docker"
url="http://kubernetes.io/"
license=("APACHE")
depends=()
makedepends=('go' 'rsync' 'git')
optdepends=('etcd: etcd cluster required to run Kubernetes'
            'mesos: requires a Mesos cluster')
conflicts=('kubernetes')
arch=('x86_64' 'i686')
install=kubernetes.install
source=("git+https://github.com/kubernetes/kubernetes"
        "kubernetes.install")
sha256sums=('SKIP'
            'f40b4b14a71f8138de69021e967d993e8b14db2cebe66eee20c7e66839ad1fde')

pkgver() {
    cd $srcdir/kubernetes
    # git describe --long | sed 's/\([^-]*-g\)/r\1/;s/-/./g'
    printf "r%s.%s" "$(git rev-list --count HEAD)" "$(git rev-parse --short HEAD)"
}

build() {
    cd $srcdir/kubernetes
    export KUBERNETES_CONTRIB=mesos
    make
}

package() {
    cd $srcdir/kubernetes

    binaries=(kube-apiserver kube-controller-manager kube-scheduler kube-proxy kubelet kubectl kubemark hyperkube
              k8sm-controller-manager k8sm-executor k8sm-scheduler km)
    for bin in "${binaries[@]}"; do
        install -Dm755 _output/local/bin/linux/amd64/$bin $pkgdir/usr/bin/$bin
    done

    # install the bash completion
    install -dm 0755 $pkgdir/usr/share/bash-completion/completions/
    install -t $pkgdir/usr/share/bash-completion/completions/ contrib/completions/bash/kubectl

    # install manpages
    install -d $pkgdir/usr/share/man/man1/
    install -pm 644 docs/man/man1/* $pkgdir/usr/share/man/man1

    # install the place the kubelet defaults to put volumes
    install -d $pkgdir/var/lib/kubelet
}
