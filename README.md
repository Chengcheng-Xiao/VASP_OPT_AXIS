# VASP_OPT_AXIS
This project provides a patch to enable the ability to fix lattice component(s) during relaxation in VASP.

This project is forked from [muchong](http://muchong.com/html/201107/3427823_2.html) and modified upon.

## Install
### original
Put `cell_relax.patch` file in the root directory of your VASP distro and type:
```
$ patch -p0 < cell_relax.patch
```
### finner
Put `cell_relax_finner.patch` file in the root directory of your VASP distro and type:
```
$ patch -p0 < cell_relax_finner.patch
```


## Usage
### original
An additional file called `OPTCELL` is needed for specifying which axis to fix.

Note that this method is reliable only in orthorhombic lattices.

OPTCELL stynax (to fix c axis):
```
110
```
Then a regulary relaxiation with `ISIF`=3 can be performed.

### finner
The finner version gives you finner control over which cell component you want to relax.

No more external file is needed. Just add `IOPTCELL` to your `INCAR` file and search for:
```
Constraining cell:
```
block in your `stdout`.

## Caveats

1. Because this method based on artificially eliminating ""pressure" on specified cell axis, it's reliable only for orthorhombic cells. Make sure you KNOW WHAT YOU ARE DOING!

## Notes

*1. VASP is a commercial package, be sure you have proper license.

*2. The correctness of this patch has been checked with few simple cases.
