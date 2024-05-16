# Maintainer: Jiri Pospisil <jiri@jpospisil.com>

pkgname=garnet
pkgver=1.0.8
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
b2sums=('c9205d227dd922d5ead886d9d38c9b0c18defd84abebda695b5084516f7d79a6a37afab2d497f60e950675f2536a4cf47579377d462a84dbcd79c457a5f02dc8'
        '3db262540ecd4c4474e5fd506ec807b80e73105415e0714cf1a33bfd4221e6722ce22c099eb83dffea8c5baf1162768804b6ba374fd6693958af9d36f51e1ebe'
        'bbb46465676585b37587ccdfbb3831367babc97cfe6ab099fc0e60878a77291ad19dd557d0851ef66b4fd987b1abcefeb3c8c2fd2d433f9458e0e76ab83b40f7')

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
  install -Dm644 -t "$pkgdir/usr/lib/garnet" GarnetServer.pdb \
    runtimes/linux-x64/native/libnative_device.so \
    CustomRespCommandsInfo.json

  mkdir "$pkgdir/usr/bin"
  ln -sr "$pkgdir/usr/lib/garnet/GarnetServer" "$pkgdir/usr/bin/GarnetServer"
}
