curl -O https://raw.github.com/jralls/gtk-osx-build/master/gtk-osx-build-setup.sh

sh gtk-osx-build-setup.sh

ln -sf $PWD/jhbuildrc-custom ~/.jhbuildrc-custom
jhbuild bootstrap && jhbuild

curl --continue-at - -L http://ftp.cc.uoc.gr/mirrors/linux/lfs/LFS/conglomeration/cups/cups-1.2.12-source.tar.bz2 -o /Users/yousseb/gtk/source/pkgs/cups-1.2.12-source.tar.bz2

