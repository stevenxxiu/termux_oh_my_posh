# Oh My Posh Package
## Termux
To build:

    $ cd build_env/
    $ ./build.nu ../packages/oh-my-posh/

In *Docker*:

    $ TERMUX=1 CARCH=aarch64 makepkg --nodeps
    $ CARCH=aarch64 makepkg --nodeps
