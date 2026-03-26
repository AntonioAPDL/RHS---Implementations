# CODEX Validation Report

## A. Repo overview
- Exact repo path: `/home/jaguir26/local/src/RHS---Implementations`
- Path mapping used: every user-specified `/data/muscat_data/jaguir26/...` path was mapped to `/home/jaguir26/local/src/...`.
- Current branch: `main`
- Git status summary (after this audit): modified `main.tex`; new `CODEX_VALIDATION_REPORT.md`; generated `main.pdf`.
- Top-level inventory:
  - `2101.00366v1.pdf`
  - `2507.10975v2.pdf`
  - `Download.pdf`
  - `nihms-1991193.pdf`
  - `main.tex`
- All `.tex` files found: `main.tex`
- All `.pdf` files found:
  - `2101.00366v1.pdf`
  - `2507.10975v2.pdf`
  - `Download.pdf`
  - `nihms-1991193.pdf`
- Scripts/notes related to horseshoe / regularized horseshoe / Gibbs: none beyond `main.tex` and the four papers.

## B. Document identification
- Main standalone note audited and edited: `main.tex`
  - Identification basis: standalone article preamble; title starts with “The Regularized Horseshoe Prior and Its Gibbs Implementations...”; full technical note structure.
- Four PDFs used for verification:
  - `Download.pdf` (Piironen–Vehtari, 2017)
  - `nihms-1991193.pdf` (Nishimura–Suchard, 2023/2024 release PDF)
  - `2507.10975v2.pdf` (Fan et al., 2025)
  - `2101.00366v1.pdf` (Bhattacharya–Khare–Pal, 2021)
- Other relevant local files used during audit: none.

## C. Conceptual validation summary
- High-level relation of papers:
  - Piironen–Vehtari (PV): original regularized horseshoe prior construction; emphasizes prior design, slab regularization, and global-scale calibration.
  - Nishimura–Suchard (NS): Gibbs-oriented reformulation using fictitious-data augmentation; preserves the **full-conditional** global-local block given `β`.
  - Fan et al.: robust Laplace/LAD model with mixture augmentation; implements horseshoe-family priors (including regularized horseshoe) in a model-specific Gibbs framework.
  - Bhattacharya–Khare–Pal (BKP): ergodicity analysis for ordinary horseshoe and two regularized variants; clarifies which conditionals are standard vs nonstandard.
- Where they agree:
  - Regularized conditional variance formula is the same core object (after notation mapping).
  - Slab scale can be fixed or random from a Bayesian modeling standpoint.
- Where they differ:
  - PV and NS are **not the same joint prior**; NS introduces a compensating factor in the local-scale structure.
  - NS preserves `p(τ,λ | β, ·)` under augmentation; PV does not preserve the ordinary global-local block.
  - Fan et al. is not a generic prior-level equivalence result; it is a robust-model-specific Gibbs construction.
- Required phrasing distinction (implemented): full-conditional equivalence vs joint-prior identity vs collapsed-update behavior.

## D. Detailed findings (section/claim review of `main.tex`)

### Verified as mathematically correct and faithful
- Unified regularized variance identity and limiting regimes.
- PV regularized horseshoe definition and product representation with Gaussian slab.
- PV recommendation of a prior on slab width (`c^2 ~ Inv-Gamma(...)`) and induced Student-t slab behavior.
- NS fictitious-data device:
  - `z_j = 0`, `z_j | β_j, ζ ~ N(β_j, ζ^2)`.
  - Unchanged full conditional for global-local block given `β`.
  - Modified `β` update with added `ζ^{-2}` precision term.
- NS ergodicity framing:
  - Main analysis with fixed `ζ`.
  - Extension to random bounded-support `ζ` (Remark 3.9).
- Fan robust-likelihood augmentation and regularized horseshoe implementation with optional inverse-gamma prior on `b^2`.
- BKP confirmation that PV-style local-scale updates are nonstandard and require rejection/Metropolis handling.

### Corrections / precision improvements made
1. Tightened “recovery” claims to explicitly mean the **full-conditional** block `p(τ,λ | β,·)`.
2. Added a dedicated notation crosswalk table (`PV`, `NS`, `Fan`) for `τ/λ_j/s_j/c/ζ/b` and likelihood-scale notation.
3. Clarified fixed-vs-random `ζ` wording:
   - replaced over-strong prose with precise statement matching NS text and Remark 3.9.
4. Added caveat separating:
   - full-conditional identity under augmentation, and
   - algebra of partially collapsed updates (where extra normalization factors can appear, as discussed in BKP).
5. Softened wording on Fan et al. “`b -> 0` not penalized” sentence:
   - retained technical correction,
   - reframed as likely wording slip, not a broad claim about model mismatch.

### Statements that were too strong or too vague and were fixed
- “Recovered Gibbs updates” was too broad; now explicitly scoped to full conditionals.
- Fixed-vs-random `ζ` statement was too categorical; now aligned to what NS explicitly proves and remarks.

### Equation-level changes
- No core formula was found mathematically incorrect in the original note.
- Changes were primarily **scope qualifiers** and **conditional/joint distinctions** to prevent over-interpretation.

### Notation consistency fixes
- Added explicit crosswalk table and reinforced distinction between:
  - Fan’s global horseshoe scale (`λ`) and
  - Fan’s likelihood scale symbol (`τ`).

## E. Key technical conclusions
1. Does the later Gibbs-oriented paper recover the ordinary horseshoe Gibbs updates?
- Yes, but specifically for the **full-conditional global-local block**.

2. If yes, for exactly which block and why?
- For `p(τ, λ | β, ...)` (and similarly local-scale conditionals in that block) under the augmented construction.
- Reason: the fictitious-data factor depends on `β` (and `ζ`) but not on `(τ, λ)` once conditioning on `β`.

3. Is that due to fixing `ζ`, or due to augmentation construction?
- Due to the augmentation / modified joint structure, **not** due to fixing `ζ`.

4. What changes when `ζ` is assigned a prior instead of being fixed?
- You add an update for `ζ` (often conjugate under inverse-gamma choice in augmentation/product forms).
- The preserved global-local block remains preserved conditionally.
- Existing ergodicity theorems in NS are directly stated for fixed `ζ` or bounded-support random `ζ`; unbounded-support priors need separate theory.

5. Is the later formulation exactly the same joint prior as the original regularized horseshoe, or only closely related?
- Only closely related; not identical joint prior.
- They share the same conditional regularized Gaussian law for `β | τ, λ, ζ`, but differ in how local-scale structure is encoded at the joint level.

6. How does the robust Gibbs paper fit into this picture?
- It is a robust, model-specific Gibbs implementation (Laplace/LAD via normal-exponential mixture) using horseshoe-family priors.
- It supports random slab treatment (`b^2`) practically.
- It does not provide a generic prior-level equivalence theorem replacing PV/NS distinctions.

## F. Compilation status
- `latexmk` status: unavailable in this environment (`latexmk: command not found`).
- `pdflatex` status: successful (multiple passes).
- Output PDF path: `/home/jaguir26/local/src/RHS---Implementations/main.pdf`
- Remaining warnings/issues:
  - Hyperref PDF-string warnings for math in section headings.
  - Two overfull hbox warnings (minor formatting, non-fatal).
  - No fatal compile errors.

## G. Remaining uncertainties
- No major unresolved mathematical contradiction remained after cross-checking the four PDFs.
- One nuance remains interpretation-sensitive in the literature:
  - whether one discusses augmented full-conditionals vs partially collapsed forms can change algebraic appearance (e.g., extra normalizing terms), even when core conditional-independence logic is intact.
- Fan et al. “`b -> 0` not penalized” appears inconsistent with their own shrinkage equations; treated here as a wording slip, but no external erratum was found in this repo.

## Optional diff summary
- `main.tex` updates were conservative and targeted:
  - added notation crosswalk table,
  - tightened conditional-vs-joint statements,
  - clarified fixed/random `ζ` and ergodicity scope,
  - added collapsed-vs-full-conditional caveat,
  - softened one over-strong wording comment while preserving the mathematical correction.
