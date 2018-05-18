# VASP_OPT_AXIS
An attempt to fix lattice constant for specific atoms during geometry optimization (relaxiation) proccess implemented in VASP pacakge.
This is very efficient for 2D materials with vacumme layer added.
Forkd from [muchong](http://muchong.com/html/201107/3427823_2.html)

## Install
Put `cell_relax.patch` file in the root directory of your VASP distro and type:
```
$ patch -p0 < cell_relax.patch
```

## Useage
An additional file called OPTCELL is needed for specifying which axis to fix.
Note that this method is reliable only in orthorhombic lattices.
OPTCELL stynax (to fix c axis):
```
110
...
```
Then a regulary relaxiation with `ISIf`=3 can be performed. 

## Caveats

1. Because this method based on artificially eliminating ""pressure" on specified cell axis, it's reliabel only on orthorhombic cells.

## Notes

*1. Because VASP is a commercial package, I cannot supply any source file.

*2. The correctness of this patch has been checked with few simple cases.

*3. This is just a trial fix, I have not (yet) get a clear picture the original intension of this interface. Any result obtained by this fix should be carefully checked by yourself.
