# Introduction

WORK in progress!

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

## Step 1: Install MSYS2

https://www.msys2.org/

## Step 2: Install required packages in MSYS64

```
pacman -Syu --needed git make mingw-w64-x86_64-toolchain mingw-w64-x86_64-wxwidgets3.0-msw mingw-w64-x86_64-openssl mingw-w64-x86_64-perl
```

Pro tip: _Add add exports to ~/.bashrc for future use_

```
export PATH=/mingw64/bin:/mingw64/bin/core_perl:/mingw64/bin/site_perl/5.32.1:$PATH
export PERL5LIB=/home/$USER/local/lib/perl5
```

Install `cpanm` so  we can easily use the upstream patches:

```
perl -S cpan install App::cpanminus
```

Install the dependencies for `Alien::wxWidgets`:

```
# install the deps to the sitelib
OPENSSL_PREFIX=/mingw64 MAKEFLAGS=-j$(nproc) cpanm --installdeps -n --verbose  Alien::wxWidgets
```

Install `Alien::wxWidgets`:

```
OPENSSL_PREFIX=/mingw64 MAKEFLAGS=-j$(nproc) cpanm -llocal -n --verbose https://github.com/orbital-transfer-example/debian-libalien-wxwidgets-perl.git@patch
```

Finally install the `Wx` module:

```
# install Wx
OPENSSL_PREFIX=/mingw64 MAKEFLAGS=-j$(nproc) cpanm -llocal -n --verbose https://github.com/orbital-transfer-example/debian-libwx-perl.git@patch
```

## Step 3: Explore `Wx` via `Wx::Demo`:

```
cpanm Wx::Demo
# Note: you may need to edit Demo.pm due to “icons” error
perl /mingw64/bin/site_perl/5.32.1/wxperl_demo.pl
```

# Installing _wxGlade_ on MSYS2

wxGlade is a program writting using _wxPython_, Python's language bindings for wxWidgets. It is
useful for Perl craftsmen because it can export Perl code based on the defined GUI. It's an excellent
way to bootstrap a `wxPerl` projects.


## Step 1: Install `wxPython`

```
pacman -Syu mingw-w64-x86_64-wxPython unzip
```

## Step 2: Download and unzip the latest version of _wxGlade_ (currently 1.0.5):


```
wget 'https://downloads.sourceforge.net/project/wxglade/wxglade/1.0.5/wxGlade-1.0.5.zip?ts=gAAAAABkZRMb57l3eOhXFgCr-fQrsv4ko96Xek4_x6M4TkaC3P9nzhkA1KabnLysHoQAHg2wnl0xhtzaH5Mih7tXHtFvq2KbaA%3D%3D&r=https%3A%2F%2Fsourceforge.net%2Fprojects%2Fwxglade%2Ffiles%2Flatest%2Fdownload' -o wxglade.zip 

unzip wxglade.zip
```

Then `cd` into the new directory and run `./wxglade.py`:

```
cd wxGlade-1.0.5

## Step 3: start wxglade
./wxglade.py
```

# Conclusion

This document doesn't cover how to actually use _wxGlade_. It's not hard, but good luck. If you would
like to contribute to this document, please do so in a pull request. It is greatly appreciated by all!
