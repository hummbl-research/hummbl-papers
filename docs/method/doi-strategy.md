# DOI Strategy — HUMMBL Papers

**Decision date:** 2026-04-16
**Status:** ACTIVE
**Pattern adopted:** **A — Per-paper Zenodo deposits**

---

## Decision

Each paper in this portfolio receives its **own Zenodo deposit** and therefore its **own concept DOI + version DOI**. Deposits are created manually via the Zenodo web UI, one per paper, at the time of publication.

The **GitHub → Zenodo webhook** on this repository is **reserved for future code/data releases only** (e.g., evaluation harnesses, benchmarks, reproducibility containers). It is **not** used as the primary DOI source for paper content. No scaffold-only tag will be minted.

---

## Why not the webhook?

Zenodo's GitHub integration mints exactly **one DOI per release of the whole repository**, not per file. For a portfolio of ~40 independent papers this produces the wrong granularity:

- Updating one paper forces a whole-repo version bump for everything else
- Readers citing the concept DOI always resolve to the latest release — for a bibliography repo, the "latest release" is ambiguous
- The concept DOI created at first tag is **permanent** and inherited by all subsequent releases; there is no retroactive split

Sources:

- Zenodo DOI versioning (official) — https://zenodo.org/help/versioning
- Zenodo GitHub integration docs — https://help.zenodo.org/docs/github/enable-repository/
- GitHub: Referencing and citing content — https://docs.github.com/articles/referencing-and-citing-content

## What the big labs actually do

AI2, DeepMind, and Stanford CRFM use arXiv + journal DOIs (via CrossRef) as the primary per-paper citation mechanism. Zenodo/GitHub integration at those orgs is generally reserved for *software artifacts* — code releases, benchmarks, datasets — not paper content. Pattern A aligns with that convention.

---

## Per-paper release workflow

For each paper ready to publish:

1. **Preprint to arXiv** (or discipline-appropriate preprint server — bioRxiv, medRxiv, SSRN). Get the arXiv ID.
2. **Create a Zenodo deposit** manually at https://zenodo.org/uploads/new
   - Use `HUMMBL Papers` as the community (if configured)
   - Upload: paper PDF, `paper.tex` source, `references.bib`, figures/, per-paper notebook
   - Metadata: title, authors (with ORCID), abstract, keywords, license
   - Add a `related_identifiers` entry pointing to the arXiv preprint
3. **Publish** the deposit. Zenodo mints the concept DOI + version DOI.
4. **Record the DOI** in three places:
   - `papers/{paper-slug}/README.md` — DOI badge + cite-as block
   - `ROADMAP.md` — Shipped table row
   - `DOI_INDEX.md` (top-level) — master mapping filename → DOI
5. **Push to arXiv** a revised version if paper changes post-DOI (arXiv handles versioning natively; Zenodo versioning creates a new version DOI under the same concept DOI).

## Per-code-release workflow (webhook path, future use)

If we ship a code/data artifact at some point (e.g., TRACE eval harness, BASE120 MCP server release, a benchmark dataset):

1. Decide: does this belong in **hummbl-papers** or a dedicated code repo?
   - Dedicated repo preferred for meaningful code releases (clean lineage)
   - hummbl-papers webhook used only for releasing companion artifacts tightly coupled to the paper portfolio
2. If using hummbl-papers webhook: tag the release on GitHub. Zenodo mints one DOI for the snapshot.
3. Record in `DOI_INDEX.md` with type = `code` or `dataset` (distinguish from paper DOIs).

---

## Irreversibility notes (read before first deposit)

- **Files are immutable once a Zenodo record is published.** New files require a new version (same concept DOI). Plan each deposit's file set carefully.
- **Metadata IS editable** post-publish (title, authors, description, license) — so a typo doesn't cost a DOI.
- **License must be declared** at deposit time; Zenodo blocks publish without one. Default for this portfolio: MIT (per repo LICENSE); individual papers may override per-deposit if content warrants a different license (e.g., CC-BY-4.0 for manuscript text while code stays MIT).
- **First deposit per paper creates that paper's concept DOI permanently.** Subsequent versions of that paper inherit the concept DOI.

---

## What hummbl-papers IS vs IS NOT

**IS:**
- Source of truth for paper drafts, notebooks, figures, reproducibility code
- Organized portfolio home (ROADMAP, method docs, per-paper directories)
- Public artifact that CITATION.cff and .zenodo.json make citable at the *portfolio* level if anyone ever wants to cite the repo itself (not the standard path)

**IS NOT:**
- The DOI source for individual papers (that's Zenodo deposits, one per paper)
- The canonical preprint venue (that's arXiv)
- A single monolithic publication (Pattern B was rejected for this reason)

---

## Open questions

- **HUMMBL community on Zenodo:** should we register a Zenodo community `hummbl-papers` or `hummbl-research` to group deposits? Community membership on Zenodo requires approval but helps readers discover the full portfolio. To be decided after first deposit.
- **Drafting vs published split:** papers currently live in `founder_mode/docs/publications/` in the founder-mode repo. When a paper is "done," does the source move to `hummbl-papers/papers/{slug}/`? Kept in both? TBD — needs a separate convention doc.

## Revision

Revise this document if:
- Zenodo changes the webhook semantics
- We decide to switch from arXiv to a different preprint server
- A partner journal mandates a specific DOI provider
- Portfolio grows beyond ~100 papers and manual deposits become infeasible (consider DataCite direct API)
