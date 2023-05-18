# Introduction

This guide is based on the best available information at the time
of its writing. At the end of this guide, you will be able to run
wxPerl applications on Windows 10 (and probably others). You will
also be able to develop applications for wxPerl using wxGlade under
the same environment.

The steps are covered in this recording of a live stream done by the
Houston Perl Mongers group in May 2023.

https://www.youtube.com/watch?v=zOW8fMrpIS0

The following is not a script, but a series of steps and commands for
installing wxPerl on MSYS2 on Windows 10, such that one is able to run
and develop (assisted by `wxGlade`) native looking Windows GUIs using
Perl and wxWidgets! (pause for dramatic affect...)

## Install MSYS2

https://www.msys2.org/

## Install required packages in MSYS64



```
pacman -Syu --needed git make mingw-w64-x86_64-toolchain mingw-w64-x86_64-wxwidgets3.0-msw mingw-w64-x86_64-openssl mingw-w64-x86_64-perl
#!! also add exports to ~/.bashrc for future use
export PATH=/mingw64/bin:/mingw64/bin/core_perl:/mingw64/bin/site_perl/5.32.1:$PATH
export PERL5LIB=/home/$USER/local/lib/perl5
perl -S cpan install App::cpanminus
# install the deps to the sitelib
OPENSSL_PREFIX=/mingw64 MAKEFLAGS=-j$(nproc) cpanm --installdeps -n --verbose  Alien::wxWidgets
# install Alien::wxWidgets
OPENSSL_PREFIX=/mingw64 MAKEFLAGS=-j$(nproc) cpanm -llocal -n --verbose https://github.com/orbital-transfer-example/debian-libalien-wxwidgets-perl.git@patch
# install Wx
OPENSSL_PREFIX=/mingw64 MAKEFLAGS=-j$(nproc) cpanm -llocal -n --verbose https://github.com/orbital-transfer-example/debian-libwx-perl.git@patch

# Wx::Demo
cpanm Wx::Demo
# may need to edit Demo.pm due to “icons” error
perl ./wxperl_demo.pl
```
