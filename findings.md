# Findings & Decisions

## Requirements
- End-to-end comparative proteomics: human DRG vs human sympathetic ganglia
- DIA mass spectrometry (dia-PASEF, timsTOF), 4 biological replicates per tissue
- All samples processed in single batch
- Identify proteins expressed in each tissue and those that differ between tissues
- Produce publication-ready manuscript and analysis report
- Python-based analysis pipeline (Claude-directed)
- **Core facility handles:** sample prep, MS acquisition, initial data processing
- **Core returns:** raw .d files, spectral counts, log2FC, protein abundance matrix
- **Our lab handles:** tissue procurement, all downstream bioinformatic analysis

## Research Findings

### Existing Datasets & Literature
- **Barry et al. 2018** — First human DRG proteome: 5,245 proteins identified (Sci Rep). Comparison with rat DRG also performed. Key reference for validating our DRG coverage.
  - URL: https://www.nature.com/articles/s41598-018-31189-9
- **Multi-omic atlas of human autonomic and sensory ganglia (2026 preprint)** — Single-cell multi-omic atlas comparing human SG and DRG. Shows major cell types are shared but key molecular differences exist between tissues. Critical reference for cross-validating our bulk proteomics findings.
  - URL: https://www.biorxiv.org/content/10.1101/2025.09.18.677119v2
- **Reference Atlas of Human DRG (2025 preprint)** — Comprehensive DRG atlas.
  - URL: https://www.biorxiv.org/content/10.1101/2025.11.05.686654v1
- **Tavares-Ferreira et al. 2022** — Spatial transcriptomics of human DRG identifying nociceptor molecular signatures (Sci Transl Med)
- **Nguyen et al. 2021** — Single-nucleus transcriptomic analysis of human DRG neurons (eLife)

### DIA Analysis Tools
- **DIA-NN** (library-free mode): Best default for DIA analysis. Fast, accurate, supports library-free/predicted spectral libraries. Recommended for this project.
  - GitHub: https://github.com/vdemichev/DiaNN
- **FragPipe/MSFragger-DIA**: Faster than DIA-NN for some datasets (6x speed). Composable, open pipeline. Good alternative.
  - URL: https://fragpipe.nesvilab.org/
- **Spectronaut**: Commercial, GUI-based, standardized exports. Not recommended for this project (cost, less flexible).
- For dia-PASEF specifically (timsTOF instruments), both DIA-NN and FragPipe have optimized workflows.
- **Instrument confirmed: Bruker timsTOF** — will use dia-PASEF acquisition. Raw files are .d format (Bruker).
- **Tissue source confirmed:** Local biorepository from organ donors.
- **Target journal confirmed:** Journal of Proteomics (Elsevier). Typical article: ~5,000-8,000 words, up to 8 figures.

### Statistical Methods for Differential Proteomics
- **DEqMS**: Built on limma, but estimates different prior variances for proteins quantified by different numbers of peptides. Gold standard for proteomics DE analysis. Recent 2026 Nature Protocols paper validates its use with DIA.
  - URL: https://www.nature.com/articles/s41596-026-01349-7
- **limma**: Works well but doesn't account for peptide-count-dependent variance.
- **proDA**: Good accuracy but very slow — not recommended for large datasets.
- **Recommended DIA workflow**: protein directLFQ intensity → median normalization or none → MinDet imputation → DEqMS/limma for DE.
- **Optimizing DE analysis (Nat Commun 2024)**: Benchmarking study found limma, ROTS, DEP, and proDA consistently top-performing. DEqMS specifically designed for MS data.

### Sample Preparation Notes
- Human ganglia should be flash-frozen and not allowed to thaw before lysis
- Lysis buffer: 20 mM NaCl, 5 mM MgCl2, 0.1% TX-100, 10 mM Tris-HCl pH 7.2 + protease inhibitors (or SDS/urea-based for proteomics)
- Dounce homogenization (5-10 strokes with loose pestle) effective for ganglia
- S-Trap or FASP digestion both suitable for small tissue amounts
- Ischemia time is critical for tissue quality — minimize time from procurement to freezing

## Technical Decisions
| Decision | Rationale |
|----------|-----------|
| DIA-NN library-free | Best sensitivity/speed balance; no need for pre-built spectral library; strong community support |
| DEqMS for DE analysis | Peptide-count-aware variance; purpose-built for proteomics; validated for DIA in 2026 Nat Protocols |
| MinDet imputation | Recommended for DIA where missingness is largely MNAR (below detection limit) |
| directLFQ quantification | Better protein quantification than MaxLFQ for DIA-NN output |
| Python + rpy2 for R packages | Keeps pipeline in Python while accessing DEqMS (R/Bioconductor) |
| Pooled QC sample | Standard practice; monitors instrument drift; enables RT alignment assessment |

## Issues Encountered
| Issue | Resolution |
|-------|------------|
| (none yet) | — |

## Resources
- DIA-NN GitHub: https://github.com/vdemichev/DiaNN
- FragPipe: https://fragpipe.nesvilab.org/
- DEqMS GitHub: https://github.com/yafeng/DEqMS
- DEqMS Nat Protocols 2026: https://www.nature.com/articles/s41596-026-01349-7
- Human DRG proteome (Barry 2018): https://www.nature.com/articles/s41598-018-31189-9
- DRG/SG multi-omic atlas (2026): https://www.biorxiv.org/content/10.1101/2025.09.18.677119v2
- UniProt human FASTA: https://www.uniprot.org/proteomes/UP000005640
- STRING database (PPI): https://string-db.org/
- g:Profiler (enrichment): https://biit.cs.ut.ee/gprofiler/
- GSEApy (Python GSEA): https://github.com/zqfang/GSEApy

## Visual/Browser Findings
- (none yet — will populate during analysis phases)

---
*Update this file after every 2 view/browser/search operations*
