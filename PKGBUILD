# Maintainer: wszqkzqk <wszqkzqk@qq.com>
pkgname=oh-my-posh
pkgver=19.20.0
pkgrel=1
pkgdesc="A prompt theme engine for any shell."
arch=('x86_64' 'armv7h' 'aarch64')
url="https://github.com/JanDeDobbeleer/oh-my-posh"
license=('MIT')
makedepends=('go' 'gcc')
depends=('glibc')
source=("$pkgname-$pkgver.tar.gz::$url/archive/v$pkgver.tar.gz")
sha256sums=('a9028c0dbe58680cbd81ad52d999917eec1b45f4379210375bb7b752bde94793')

if [ -z "$TERMUX" ]; then
    TERMUX_PREFIX='/usr'
else
    TERMUX_PREFIX='/data/data/com.termux/files/usr'
fi

build() {
    export CGO_CPPFLAGS="${CPPFLAGS}"
    export CGO_CFLAGS="${CFLAGS}"
    export CGO_CXXFLAGS="${CXXFLAGS}"
    export CGO_LDFLAGS="${LDFLAGS}"
    export GOFLAGS="-buildmode=pie -trimpath -mod=readonly -modcacherw"

    if [ $CARCH = 'aarch64' ]; then
        export CGO_ENABLED=1
        export CC=/opt/android-ndk/toolchains/llvm/prebuilt/linux-x86_64/bin/aarch64-linux-android34-clang
        export GOOS=android
        export GOARCH=arm64
        export GOARM=7
    fi

    cd "$pkgname-$pkgver/src"
    local ldflags=''
    ldflags+="-linkmode=external "
    ldflags+="-X github.com/jandedobbeleer/oh-my-posh/src/build.Version=$pkgver "
    ldflags+="-X github.com/jandedobbeleer/oh-my-posh/src/build.Date=$(date +%F)"
    go build -buildvcs=false -ldflags="$ldflags" -o "$pkgname"

    if [ $CARCH != 'aarch64' ]; then
        "./${pkgname}" completion bash > "${pkgname}.sh"
        "./${pkgname}" completion fish > "${pkgname}.fish"
        "./${pkgname}" completion zsh > "_${pkgname}"
    fi
}

package() {
    cd "$pkgname-$pkgver/src"
    install -Dm 755 ./"${pkgname}" "${pkgdir}${TERMUX_PREFIX}/bin/oh-my-posh"
    install -Dm 644 "../COPYING" "${pkgdir}${TERMUX_PREFIX}/share/licenses/${pkgname}/LICENSE"
    install -d "${pkgdir}${TERMUX_PREFIX}/share/oh-my-posh/themes"
    install -m 644 ../themes/* -t "${pkgdir}${TERMUX_PREFIX}/share/oh-my-posh/themes"

    if [ $CARCH != 'aarch64' ]; then
        install -Dm 644 "${pkgname}.sh" "${pkgdir}${TERMUX_PREFIX}/share/bash-completion/completions/${pkgname}"
        install -Dm 644 "${pkgname}.fish" "${pkgdir}${TERMUX_PREFIX}/share/fish/completions/${pkgname}.fish"
        install -Dm 644 "_${pkgname}" "${pkgdir}${TERMUX_PREFIX}/share/zsh/site-functions/_${pkgname}"
    fi
}
