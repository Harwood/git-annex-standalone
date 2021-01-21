# Maintainer: Thomas Jost <schnouki@schnouki.net>
pkgname=git-annex-standalone
pkgver=8.20200523
pkgrel=1
pkgdesc="Manage files with git, without checking their contents into git. Standalone version, with no Haskell dependency."
arch=('aarch64' 'x86_64')
url="https://git-annex.branchable.com/"
license=('GPL')
depends=("file" "git" "libffi" "lsof" "rsync" "sqlite")
optdepends=()
provides=("git-annex")
conflicts=("git-annex")

source_aarch64=("https://downloads.kitenet.net/git-annex/linux/current/git-annex-standalone-arm64.tar.gz"{,.info}{,.sig})
source_x86_64=("https://downloads.kitenet.net/git-annex/linux/current/git-annex-standalone-amd64.tar.gz"{,.info}{,.sig})

pkgver() {
  awk 'NR==4' git-annex-standalone-*.tar.gz.info
}

validpgpkeys=("40055C6AFD2D526B2961E78F5EE1DBA789C809CB")

package() {
  cd "$srcdir/git-annex.linux"

  for lib in $(ls lib/*-linux-gnu/libffi.so.*); do
    install -Dm644 $lib "$pkgdir/usr/share/$pkgname/lib/$lib"
  done

  for exe in git-annex git-annex-shell; do
    install -Dm755 shimmed/$exe/$exe "$pkgdir/usr/share/$pkgname/bin/$exe"
    install -d "$pkgdir/usr/bin"
    cat > "$pkgdir/usr/bin/$exe" <<EOF
#!/bin/bash
export LD_LIBRARY_PATH="/usr/share/$pkgname/lib":\$LD_LIBRARY_PATH
exec "/usr/share/$pkgname/bin/$exe" "\$@"
EOF
    chmod +x "$pkgdir/usr/bin/$exe"
  done

  install -Dm644 LICENSE "$pkgdir/usr/share/licenses/$pkgname/LICENSE"
  install -Dm644 logo.svg "$pkgdir/usr/share/pixmaps/git-annex.svg"
  install -Dm644 logo_16x16.png "$pkgdir/usr/share/pixmaps/git-annex_16x16.png"

  for f in usr/share/man/man1/*.1; do
    install -Dm644 $f "$pkgdir/$f"
  done
}

md5sums_aarch64=('2becc4d5cc61bbc80ea645a12085f6ce'
                 'SKIP'
                 '91267b3a38146f13ff7a9c7a877569f1'
                 'SKIP')
md5sums_x86_64=('aed16a47d00e11d2e93b3891f9912191'
                'SKIP'
                'a0587c65fc3131b918e3b9c712413a8d'
                'SKIP')
