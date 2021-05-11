# VASP OPT AXIS
This project provides patches enabling the ability to fix lattice vector component(s)/stress tensor component(s) during relaxation in VASP.

__PLEASE READ THROUGH THIS FILE BEFORE PROCEED__

## Stress tensor method
This method is forked from [muchong](http://muchong.com/html/201107/3427823_2.html) and modified upon.

The lattice vectors are updated after each SCF loop by following formula (you can find codes similar to the following in `dyna.F`):

```
      DO J=1,3
         DO I=1,3
            A(I,J)=AC(I,J)
            DO K=1,3
               A(I,J)=A(I,J) + SSIF(I,K)*AC(K,J)*STEP
            ENDDO
         ENDDO
      ENDDO
```

Where `SSIF` is the (scaled) stress tensor (note that fortran uses Column-major order), `A` is the lattice vector matrix and `STEP` is the step size.

__NOTE: This means that fixing the stress tensor elements does not necessarily fix the corresponding lattice components.__

### Fixing only the diagonal term

Put `stress_relax.patch` file in the root directory of your VASP distro. and type:
```
$ patch -p0 < stress_relax.patch
```
and recompile VASP.

This patch lets VASP read a file called `OPTCELL` after each SCF loop and use the info inside to determine which diagonal stress component(s) to fix.

`OPTCELL` file format:

```
xx yy zz
```

For example:
```
1 1 0
```
will fix `zz` component of the stress tensor.

Note that for this patch we can only fix the diagonal stress tensor element(s) so it's better suited for orthorhombic cell or at least lattice vector that is perpendicular to other lattice vectors.

Also note that `ISIF=3` is needed for lattice relaxation.

### Fixing specific stress tensor element(s)

Put `stress_relax_finner.patch` file in the root directory of your VASP distro. and type:
```
$ patch -p0 < stress_relax_finner.patch
```
and recompile VASP.

This patch lets VASP read `IOPTCELL` tag directly from `INCAR`.

`IOPTCELL` format:

```
IOPTCELL = xx yx zx xy yy zy xz yz zz
```
for example:
```
IOPTCELL = 1 1 0 1 1 0 0 0 0
```
will relax the `xx`, `xy`, `yx` and `yy` components of the stress tensor, while keeping the other components fixed (to zero).

## Direct fixing of the lattice method
__WARNINIG: this method only works with `IBRION=2` and may induce numerical instabilities.__

This patch enables direct fixing of the lattice.

This is achieved by re-assigning the original value of the lattice elements after each geometry updates of the conjugate gradient step. (modification made in `dyna.F`)

Also, the stress tensor is modified to reduce instabilities introduced by direct fixing. (modification made in `constr_cell_relax.F`).

To use this method:

Put `cell_relax.patch` file in the root directory of your VASP distro. and type:
```
$ patch -p0 < cell_relax.patch
```
and recompile VASP.

This patch lets VASP read `IOPTCELL` tag directly from `INCAR`.

`IOPTCELL` format:

```
IOPTCELL = xx xy xz yx yy yz zx zy zz
```
for example:
```
IOPTCELL = 1 1 0 1 1 0 0 0 0
```
will relax the `xx`, `xy`, `yx` and `yy` components of the lattice matrix, while keeping the other components fixed.
It will also fix the `xz`, `yz` and `zz` components of the stress tensor.

## Disclaimer

*1. VASP is a commercial package, be sure you have a proper license to use it.

*2. The correctness of this patch has been checked with few simple cases. However, this is not guaranteed to work for your specific system. Make sure you KNOW WHAT YOU ARE DOING!
