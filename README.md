# Unirational_CI

Magma scripts accompanying the paper on unirationality of complete intersections via complete osculating towers.

The scripts implement the numerical recursions and computational tests used in the paper. They are meant as reproducibility tools and computational checks; the proofs are contained in the paper.

## Contents

The repository contains four main types of scripts.

## 1. Effective bounds for complete intersections

The first script computes the effective osculating-tower bound for a general complete intersection.

The input is:

n;
c;
degs := [d1, ..., dc];

where `n` is the ambient projective dimension, `c` is the codimension, and `degs` is the multidegree of the complete intersection.

The script returns the bound produced by the osculating-tower method and checks whether the given value of `n` satisfies it. It also prints the residual multidegree chain obtained by iterating the transform `T`.

Typical commands:

OsculatingTowerBound(133, 1, [6], 0, 0, true);
OsculatingTowerBound(2069, 1, [7], 0, 0, true);
OsculatingTowerBound(81800, 1, [8], 0, 0, true);
OsculatingTowerBound(100, 2, [2,4], 0, 0, true);

With `MaxExtraR = 0` and `MaxExtraM = 0`, the script uses the canonical choices appearing in the paper. Increasing `MaxExtraR` and `MaxExtraM` searches nearby choices of contact dimensions and osculating extensions.

## 2. Effective bounds for hypersurfaces

The second script is the hypersurface specialization of the recursion. Given a degree `d`, it returns the effective bound `N_iosc(d)`.

Typical commands:

PrintHypersurfaceBound(6);
PrintHypersurfaceBound(7);
PrintHypersurfaceBound(8);
PrintHypersurfaceBound(9);

Expected values include:

N_iosc(6) = 133
N_iosc(7) = 2069
N_iosc(8) = 81800
N_iosc(9) = 719127963

Thus the script verifies the effective bounds used in the paper for low degrees.

## 3. Comparison with previous bounds

The third script compares the osculating-tower bound with previous bounds in the hypersurface case.

It computes:

- Ramero's effective bound;
- the Beheshti--Riedl bound `2^(d!)`;
- Cheng's effective bound;
- Cheng's closed estimate `2^((d-1)*2^(d-5))`, for `d >= 6`;
- the effective osculating-tower bound `N_iosc(d)`;
- the closed osculating-tower estimate `2^((d-2)*2^(d-5))`, for `d >= 6`.

Typical commands:

CompareHypersurfaceBounds(6);
CompareHypersurfaceBounds(7);
CompareHypersurfaceBounds(8);
CompareRange(6,9);

For `d = 6, 7, 8`, the script recovers:

Cheng effective: 160, 20376, 11914188890
Our effective:   133, 2069, 81800

The script also prints ratios and logarithmic comparisons, so that the size of the improvement can be read directly from the output.

## 4. Explicit sextic test

The fourth script implements the first nontrivial case of the construction, namely sextic hypersurfaces.

It constructs a sparse random sextic `F` in `P^133` over `GF(1009)` inside a fixed osculating chart

L0 = P^4 subset M0 = P^6.

The script checks the osculation condition

F|_{M0} in I_{L0}^5(6),

and implements the residual tower

(6) -> (2,3,4) -> (2,2,2,3) -> (2,2,2).

It then produces points on the sextic by the residual parametrization and verifies that each generated point satisfies

Evaluate(F, Eltseq(P)) eq 0;

The script also generates many parametrized points and tests the rank of their linear span in the ambient vector space. Reaching rank `134` means that the generated points span the full ambient projective space `P^133`.

Typical output includes:

Osculation condition: true
F(P) = 0
Success: the parametrization produced a point on the sextic.
Final vector-space rank: 134
Success: the generated points span the whole ambient P^133.

## Requirements

The scripts are written for Magma.

They use only standard Magma functionality: polynomial rings, finite fields, vector spaces, linear algebra, and polynomial evaluation.

## Important note on the sextic script

The sextic script does not construct a dense random sextic in all monomials of `P^133`. Such a polynomial is far too large for practical computation.

Instead, it constructs a random sparse sextic inside the osculating chart required by the proof. This is the computational analogue of fixing a point of the tower incidence. The script is therefore intended to test the residual construction and the numerical behavior of the parametrization, not to sample arbitrary dense sextics.

## Purpose

The purpose of the repository is to make the numerical recursion and the explicit sextic construction reproducible.

The scripts are not used as a substitute for the proofs in the paper; they provide independent computational checks of the bounds and of the first explicit case of the construction.
