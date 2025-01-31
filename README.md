# NMatrix

[![CI](https://github.com/b08x/nmatrix/actions/workflows/rubygem.yml/badge.svg?branch=development)](https://github.com/b08x/nmatrix/actions/workflows/rubygem.yml)

<img src="https://badges.gitter.im/SciRuby/nmatrix.svg" alt="Join the chat at
https://gitter.im/SciRuby/nmatrix">](https://gitter.im/SciRuby/nmatrix?utm_sou
rce=badge&utm_medium=badge&utm_campaign=pr-badge&utm_content=badge)

Fast Numerical Linear Algebra Library for Ruby

*   [sciruby.com](http://sciruby.com)
*   [Google+](https://plus.google.com/109304769076178160953/posts)
*   [Google Group - Mailing
    List](https://groups.google.com/forum/#!forum/sciruby-dev)
*   [NMatrix Installation
    wiki](https://github.com/SciRuby/nmatrix/wiki/Installation)
*   [SciRuby Installation guide](http://sciruby.com/docs#installation)


<img
src=https://travis-ci.org/SciRuby/nmatrix.png>](https://travis-ci.org/SciRuby/
nmatrix)

<img src="https://codeclimate.com/github/SciRuby/nmatrix.png"
/>](https://codeclimate.com/github/SciRuby/nmatrix)

## Description

NMatrix is a fast numerical linear algebra library for Ruby, with dense and
sparse matrices, written mostly in C and C++ (and with experimental JRuby
support). It is part of the SciRuby project.

NMatrix was inspired by [NArray](http://narray.rubyforge.org), by Masahiro
Tanaka.

Several gems are provided in this repository:
*   `nmatrix`
*   `nmatrix-java`
*   `nmatrix-atlas`
*   `nmatrix-lapacke`
*   `nmatrix-fftw`


## Installation

To install the latest stable version:

    gem install nmatrix

NMatrix was originally written in C/C++, but an experimental JRuby version is
also included (see instructions below for JRuby). For the MRI (C/C++) version,
you need:
*   Ruby 2.0 or later
*   a compiler supporting C++11 (clang or GCC)


To install the `nmatrix-atlas` or `nmatrix-lapacke` extensions, an additional
requirement is a compatible LAPACK library. Detailed directions for this step
can be found [here](https://github.com/SciRuby/nmatrix/wiki/Installation).

If you want to obtain the latest (development) code, you should generally do:

    git clone https://github.com/SciRuby/nmatrix.git
    cd nmatrix/
    gem install bundler
    bundle install
    bundle exec rake compile
    bundle exec rake spec

If you want to try out the code without installing:

    bundle exec rake pry

To install:

    bundle exec rake install

### JRuby

First, you need to download Apache Commons Math 3.6.1 (the JAR, which you can
find in the binary package). For example, in the NMatrix directory, do:

    wget https://www.apache.org/dist/commons/math/binaries/commons-math3-3.6.1-bin.tar.gz
    tar zxvf commons-math3-3.6.1-bin.tar.gz
    mkdir ext/nmatrix_java/vendor/
    cp commons-math3-3.6.1/commons-math3-3.6.1.jar ext/nmatrix_java/vendor/

Next, create build directories:

    mkdir -p ext/nmatrix_java/build/class
    mkdir ext/nmatrix_java/target

Finally, compile and package as jar.

    rake jruby

### Plugins

The commands above build and install only the core `nmatrix` gem.  If you want
to build one or more of the plugin gems (`nmatrix-atlas`, `nmatrix-lapacke`)
in addition to the core nmatrix gem, use the `nmatrix_plugins=` option, e.g.
`rake compile nmatrix_plugins=all`, `rake install nmatrix_plugins=atlas`,
`rake clean nmatrix_plugins=atlas,lapacke`. Each of these commands apply to
the `nmatrix` gem and any additional plugin gems specified. For example, `rake
spec nmatrix_plugins=atlas` will test both the core `nmatrix` gem and the
`nmatrix-atlas` gem.

### Upgrading from NMatrix 0.1.0

If your code requires features provided by ATLAS (Cholesky decomposition,
singular value decomposition, eigenvalues/eigenvectors, inverses of matrices
bigger than 3-by-3), your code now depends on the `nmatrix-atlas` gem. You
will need to add this a dependency of your project and `require
'nmatrix/atlas'` in addition to `require 'nmatrix'`. In most cases, no further
changes should be necessary, however there have been a few [API
changes](https://github.com/SciRuby/nmatrix/wiki/API-Changes), please check to
see if these affect you.

## Documentation

If you have a suggestion or want to add documentation for any class or method
in NMatrix, please open an issue or send a pull request with the changes.

You can find the complete API documentation [on our
website](http://sciruby.com/nmatrix/docs/).

## Examples

Create a new NMatrix from a ruby Array:

    >> require 'nmatrix'
    >> NMatrix.new([2, 3], [0, 1, 2, 3, 4, 5], dtype: :int64)
    => [
        [0, 1, 2],
        [3, 4, 5]
       ]

Create a new NMatrix using the `N` shortcut:

    >> m = N[ [2, 3, 4], [7, 8, 9] ]
    => [
        [2, 3, 4],
        [7, 8, 9]
       ]
    >> m.inspect
    => #<NMatrix:0x007f8e121b6cf8shape:[2,3] dtype:int32 stype:dense>

The above output requires that you have a pretty-print-enabled console such as
Pry; otherwise, you'll see the output given by `inspect`.

If you want to learn more about how to create a matrix, [read the guide in our
wiki](https://github.com/SciRuby/nmatrix/wiki/How-to-create-an-NMatrix).

Again, you can find the complete API documentation [on our
website](http://sciruby.com/nmatrix/docs/).

### Using advanced features provided by plugins

Certain features (see the documentation) require either the nmatrix-atlas or
the nmatrix-lapacke gem to be installed. These can be accessed by using
`require 'nmatrix/atlas'` or `require 'nmatrix/lapacke'`. If you don't care
which of the two gems is installed, use `require 'nmatrix/lapack_plugin'`,
which will require whichever one of the two is available.

Fast fourier transforms can be conducted with the nmatrix-fftw extension,
which is an interface to the FFTW C library. Use `require 'nmatrix/fftw'` for
using this plugin.

## Plugin details

### ATLAS and LAPACKE

The `nmatrix-atlas` and `nmatrix-lapacke` gems are optional extensions of the
main `nmatrix` gem that rely on external linear algebra libraries to provide
advanced features for dense matrices (singular value decomposition,
eigenvalue/eigenvector finding, Cholesky factorization), as well as providing
faster implementations of common operations like multiplication, inverses, and
determinants. `nmatrix-atlas` requires the [ATLAS
library](http://math-atlas.sourceforge.net/), while `nmatrix-lapacke` is
designed to work with various LAPACK implementations (including ATLAS). The
`nmatrix-atlas` and `nmatrix-lapacke` gems both provide similar interfaces for
using these advanced features.

### **FFTW**

This is plugin for interfacing with the [FFTW library](http://www.fftw.org).
It has been tested with FFTW 3.3.4.

It works reliably only with 64 bit numbers (or the NMatrix `:float64` or
`:complex128` data type). See the docs for more details.

## NArray compatibility

When [NArray](http://masa16.github.io/narray/) is installed alongside NMatrix,
`require 'nmatrix'` will inadvertently load NArray's `lib/nmatrix.rb` file,
usually accompanied by the following error:

    uninitialized constant NArray (NameError)

To make sure NMatrix is loaded properly in the presence of NArray, use
`require 'nmatrix/nmatrix'` instead of `require 'nmatrix'` in your code.

## Developers

Read the instructions in `CONTRIBUTING.md` if you want to help NMatrix.

## Features

The following features exist in the current version of NMatrix (0.1.0.rc1):

*   Matrix and vector storage containers: dense, yale, list (more to come)
*   Data types: byte (uint8), int8, int16, int32, int64, float32, float64,
    complex64, complex128, Ruby object
*   Interconversion between storage and data types
*   Element-wise and right-hand-scalar operations and comparisons for all
    matrix types
*   Matrix-matrix multiplication for dense (with and without ATLAS) and yale
*   Matrix-vector multiplication for dense (with and without ATLAS)
*   Lots of enumerators (each, each_with_indices, each_row, each_column,
    each_rank, map, etc.)
*   Matrix slicing by copy and reference (for dense, yale, and list)
*   Native reading and writing of dense and yale matrices
    *   Optional compression for dense matrices with symmetry or
        triangularity: symmetric, skew, hermitian, upper, lower

*   Input/output:
    *   Matlab .MAT v5 file input
    *   MatrixMarket file input/output
    *   Harwell-Boeing and Fortran file input
    *   Point Cloud Library PCD file input

*   C and C++ API
*   BLAS internal implementations (no library) and external (with
    nmatrix-lapack or nmatrix-atlas) access:
    *   Level 1: xROT, xROTG (BLAS dtypes only), xASUM, xNRM2, IxAMAX, xSCAL
    *   Level 2: xGEMV
    *   Level 3: xGEMM, xTRSM

*   LAPACK access (with nmatrix-lapack or nmatrix-atlas plugin):
    *   xGETRF, xGETRI, xGETRS, xGESV (Gaussian elimination)
    *   xPOTRF, xPOTRI, xPOTRS, xPOSV (Cholesky factorization)
    *   xGESVD, xGESDD (singular value decomposition)
    *   xGEEV (eigenvalue decomposition of asymmetric square matrices)

*   LAPACK-less internal implementations (no plugin or LAPACK needed and
    working on non-BLAS dtypes):
    *   xGETRF, xGETRS

*   LU decomposition
*   Matrix inversions
*   Determinant calculation for BLAS dtypes
*   Traces
*   Ruby/GSL interoperability (requires [SciRuby's fork of
    rb-gsl](http://github.com/SciRuby/rb-gsl))
*   slice assignments, e.g.,
        x[1..3,0..4] = some_other_matrix


### Planned features (Short-to-Medium Term)

See the issues tracker for a list of planned features or to request new ones.

## License

Copyright (c) 2012--17, John Woods and the Ruby Science Foundation.

All rights reserved.

NMatrix, along with SciRuby, is licensed under the BSD 2-clause license. See
[LICENSE.txt](https://github.com/SciRuby/sciruby/wiki/License) for details.

## Donations

Support a SciRuby Fellow:

[<img
src=http://pledgie.com/campaigns/15783.png?skin_name=chrome>](http://www.pledg
ie.com/campaigns/15783)
