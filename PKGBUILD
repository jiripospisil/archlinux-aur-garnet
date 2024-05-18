# Maintainer: Jiri Pospisil <jiri@jpospisil.com>

pkgname=garnet
pkgver=1.0.10
pkgrel=1
pkgdesc='A high-performance cache-store from Microsoft Research'
arch=('x86_64')
url='https://microsoft.github.io/garnet'
license=('MIT')
_dotnet_ver=8.0
depends=("dotnet-runtime-$_dotnet_ver")
makedepends=("dotnet-sdk-$_dotnet_ver")
options=('!strip' '!debug')
backup=('etc/garnet/garnet-server.conf')
source=(
  "https://github.com/microsoft/garnet/archive/refs/tags/v$pkgver.tar.gz"
  'garnet-server.service'
  'garnet-server.conf'
)
b2sums=('0a2fba1e81c224c2c03b60b5cf47c41ce15549cdb1eb228ba313037654c062b9585dc235f51dd82cc8cd20e66982c3bbee84075ec03cacb4523786f3b697be7b'
        '3db262540ecd4c4474e5fd506ec807b80e73105415e0714cf1a33bfd4221e6722ce22c099eb83dffea8c5baf1162768804b6ba374fd6693958af9d36f51e1ebe'
        '58198bd631ba26b4df777e8b748853a27cc930a848ea355c8062b8b316a7de0fb9d2eb2166be3bcb2ff69042e51c1c0ae09f12a9b47423bdc4454ea8ff870637')

build() {
  cd "$srcdir/garnet-$pkgver/main/GarnetServer"

  export DOTNET_CLI_TELEMETRY_OPTOUT=1
  export DOTNET_SKIP_FIRST_TIME_EXPERIENCE=1
  export DOTNET_NOLOGO=1

  dotnet publish GarnetServer.csproj -o build -p:PublishProfile=linux-x64-based "-f:net$_dotnet_ver"
}

package() {
  install -Dm644 -t "$pkgdir/usr/lib/systemd/system" garnet-server.service
  install -Dm644 -t "$pkgdir/etc/garnet" garnet-server.conf

  cd "$srcdir/garnet-$pkgver/main/GarnetServer/build"

  mkdir "$pkgdir/usr/lib/garnet"
  install -Dm755 -t "$pkgdir/usr/lib/garnet" GarnetServer
  install -Dm644 -t "$pkgdir/usr/lib/garnet" GarnetServer.pdb runtimes/linux-x64/native/libnative_device.so

  mkdir "$pkgdir/usr/bin"
  ln -sr "$pkgdir/usr/lib/garnet/GarnetServer" "$pkgdir/usr/bin/GarnetServer"
}
