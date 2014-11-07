# TeaLeaf ![image](Tea3D_alpha_small.png "TeaLeaf")

TeaLeaf is a mini-app that solves the linear heat conduction equation on a
spatially decomposed regularly grid using a 5 point stencil with implicit
solvers. TeaLeaf currently solves the equations in two dimensions, but three
dimensional support is in beta.

In TeaLeaf temperatures are stored at the cell centres. A conduction
coefficient is calculated that is equal to the cell centred density or the
reciprocal of the density. This is then averaged to each face of the cell for
use in the solution. The solve is carried out using an implicit method due to
the severe timestep limitations imposed by the stability criteria of an
explicit solution for a parabolic partial differential equation. The implicit
method requires the solution of a system of linear equations which form a
regular sparse matrix with a well defined structure.

The computation in TeaLeaf has been broken down into "kernels", low level
building blocks with minimal complexity. Each kernel loops over the entire grid
and updates the relevant mesh variables. Control logic within each kernel is
kept to a minimum, allowing maximum optimisation by the compiler. Memory is
sacrificed in order to increase performance, and any updates to variables that
would introduce dependencies between loop iterations are written into copies of
the mesh.

Normally, third party solvers are used to invert the system of linear
equations, because of the complexity of state of the art methods. For reference
the simplest iterative method, the Jacobi method, has been included in the
reference version as the default as well the option to use a Conjugate Gradient or a Chebyshev solver.
These does have the advantage of being matrix free and independent of library dependencies.
By deault a pre-conditioner is not invoked, but a simple pre-conditioner is available as an
option which just uses diagonal scaling. Note that this simple method will not always speed up the solve time.

The solvers have been written in Fortran with OpenMP and MPI and they have also
been ported to OpenCL to provide an accelerated capability.

Other versions invoke third party linear solvers and currently include
Petsc, Trilinos and Hypre, which are in beta release. For each of these version there are instructions on
how to download, build and link in the relevant library.

## Current Implementations:

- Fortran with OpenMP and MPI
- OpenCL
- Petsc
- Trilinos
- Hypre

## Coming Soon:

- CUDA and AMGX
