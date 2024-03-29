# Maintainer: Jiri Pospisil <jiri@jpospisil.com>

pkgname=garnet
pkgver=1.0.2
pkgrel=1
pkgdesc='A high-performance cache-store from Microsoft Research'
arch=('x86_64')
url='https://microsoft.github.io/garnet'
license=('MIT')
_dotnet_ver=8.0
makedepends=("dotnet-sdk-$_dotnet_ver")
depends=("dotnet-runtime-$_dotnet_ver")
backup=('etc/garnet/garnet-server.conf')
source=(
  "https://github.com/microsoft/garnet/archive/refs/tags/v$pkgver.tar.gz"
  'garnet-server.service'
  'garnet-server.conf'
)
b2sums=('5f1cda0a8841be8ed9ec2eb4c67bcfa213f6b20a35f62cea4452fcc1ca32e569809bb0b0a766ab20eea0d4790329522ceb896e5c931c12c509d8dfb78fe1b3db'
        'b6c13c56736a6114d29f8e929dba82dbcc951e329f225171a5718aa5eb93488d56295d29eb6bf1a72090ff8122305b48745287a594f4e5eac940ecbbc6da16f2'
        '6917ef89ca33ba4f3b3019c63ca222bc78afafb67ac68e856391aeab6170cc89d384109d3951677cb9eb970a5012e8d22c753fcb676e527b5bb8f852d4fbbbb5')

build() {
  cd "$srcdir/garnet-$pkgver/main/GarnetServer"

  export DOTNET_CLI_TELEMETRY_OPTOUT=1
  export DOTNET_SKIP_FIRST_TIME_EXPERIENCE=1
  export DOTNET_NOLOGO=1

  dotnet publish -c Release -o build -r linux-x64 --sc false -f "net$_dotnet_ver"
}

package() {
  install -Dm644 -t "$pkgdir/usr/lib/systemd/system" garnet-server.service
  install -Dm644 -t "$pkgdir/etc/garnet" garnet-server.conf

  cd "$srcdir/garnet-$pkgver/main/GarnetServer"

  mkdir -p "$pkgdir/usr/"{lib,bin}
  cp -r build "$pkgdir/usr/lib/garnet"

  ln -sr "$pkgdir/usr/lib/garnet/GarnetServer" "$pkgdir/usr/bin/GarnetServer"
  chmod 755 "$pkgdir/usr/lib/garnet/GarnetServer"
}
