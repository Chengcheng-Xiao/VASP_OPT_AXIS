# VASP_OPT_AXIS
This project provides patches enabling the ability to fix lattice component(s) during relaxation in VASP.

This project is forked from [muchong](http://muchong.com/html/201107/3427823_2.html) and modified upon.

## Install
### Original version
__The original version allows you to control which vector(s) to relax.__

Put `cell_relax.patch` file in the root directory of your VASP distro. and type:
```
$ patch -p0 < cell_relax.patch
```
### Finner version
__The finner version gives you finner control over which cell component(s) you want to relax.__

Put `cell_relax_finner.patch` file in the root directory of your VASP distro. and type:
```
$ patch -p0 < cell_relax_finner.patch
```


## Usage
### Original version
An additional file called `OPTCELL` is needed to specify which axis we want fix.

Note that this method is reliable only for orthorhombic lattices.

Syntax for `OPTCELL` file (fixing the `c` axis):
```
110
```
Then a regular relaxation with `ISIF=3` can be performed.

### Finner version
No more external file is needed. Just add `IOPTCELL` to your `INCAR` file and search for:
```
Constraining cell:
```
block in your `stdout`.

`IOPTCELL` syntax:
```
IOPTCELL = xx xy xz yx yy yz zx zy zz
```
for example:
```
IOPTCELL = 1 1 0 1 1 0 0 0 0
```
will relax the `xx`, `xy`, `yx` and `yy` components, while keeping the other components fixed.

Then a regular relaxation with `ISIF=3` can be performed.

__Note:__ setting components to 0 will fix those components. Any other integer means that specific component will be relaxed.

<!-- ## Caveats

1. [Not sure] Because this method based on artificially eliminating ""pressure" on specified cell axis, it's reliable only for orthorhombic cells. Make sure you KNOW WHAT YOU ARE DOING! -->

## Notes

*1. VASP is a commercial package, be sure you have a proper license to use it.

*2. The correctness of this patch has been checked with few simple cases. However, this is not guaranteed to work for your specific case. Make sure you KNOW WHAT YOU ARE DOING!
