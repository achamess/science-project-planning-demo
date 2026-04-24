# Task Plan: DRG vs Sympathetic Ganglia Comparative Proteomics

## Goal
Perform an end-to-end comparative proteomics analysis of human dorsal root ganglia (DRG) and sympathetic ganglia (SG) using DIA mass spectrometry to identify tissue-specific and differentially expressed proteins, producing a publication-ready manuscript and analysis report.

## Current Phase
Phase 1

## Experimental Design Summary
- **Tissues:** Human DRG and human sympathetic ganglia
- **Replicates:** 4 biological replicates per tissue (8 samples total)
- **Acquisition:** dia-PASEF on Bruker timsTOF (core facility)
- **Batch design:** All samples processed together (single batch)
- **Tissue source:** Local biorepository, organ donors
- **Instrument:** Bruker timsTOF (dia-PASEF) — core facility
- **Target journal:** Journal of Proteomics

## Division of Labor
- **Our lab:** Tissue procurement, dissection, flash-freezing → hand off to core
- **Core facility:** Sample prep (lysis, digestion, cleanup), LC-MS/MS acquisition, initial data processing
- **Returns to us:** Raw .d files + processed data (spectral counts, log2FC, protein lists)
- **Our analysis (this plan):** QC validation, independent statistical analysis, enrichment, networks, figures, manuscript

---

## Phases

### Phase 1: Tissue Procurement & Core Submission
- [ ] Source human DRG and sympathetic ganglia tissue from local biorepository (organ donors)
- [ ] Confirm IRB/ethics approval and tissue use agreements
- [ ] Dissect and flash-freeze tissue samples (4 DRG + 4 SG)
- [ ] Document donor demographics (age, sex, cause of death, PMI)
- [ ] Submit tissue to proteomics core facility with experimental design document
- [ ] Confirm core facility protocol: dia-PASEF acquisition, processing pipeline, deliverables
- [ ] Request from core: raw .d files, protein abundance matrix, spectral count table, search parameters used
- **Status:** pending
- **Key decisions:** Tissue handling protocol, metadata to collect per donor
- **Deliverables:** 8 tissue samples submitted, donor metadata table, core submission form

### Phase 2: Data Receipt & Inventory
- [ ] Receive data package from core facility
- [ ] Inventory deliverables: raw files, processed matrices, search reports
- [ ] Document core's processing parameters (software, FASTA, FDR, normalization)
- [ ] Organize files: raw data → `data/raw/`, core outputs → `data/processed/core_output/`
- [ ] Assess whether core-provided log2FC is usable or if we need to reprocess from protein abundances
- [ ] Decide: use core's quantification as-is, reprocess from their abundance matrix, or reprocess from raw
- **Status:** pending
- **Key decisions:** Whether to accept core's processing or reprocess independently
- **Deliverables:** Organized data directory, data inventory document, processing decision

### Phase 3: Quality Control & Data Cleaning
- [ ] Load protein abundance matrix into Python (pandas)
- [ ] Assess data dimensions: how many proteins quantified, how many per sample
- [ ] Assess missing value patterns across samples and conditions
- [ ] Filter: remove contaminants, reverse hits, single-peptide IDs (if not already done by core)
- [ ] Evaluate replicate correlation (Pearson r, CV distributions)
- [ ] PCA / unsupervised clustering to check sample grouping and detect outliers
- [ ] Assess normalization needs (median normalization, quantile, or none — depends on what core already did)
- [ ] Apply missing value imputation if needed (MinDet or MinProb for DIA)
- [ ] Generate QC report with all metrics and visualizations
- **Status:** pending
- **Key decisions:** Filtering thresholds, normalization method, imputation strategy
- **Deliverables:** Cleaned protein matrix, QC report (PDF/HTML), outlier assessment

### Phase 4: Differential Expression Analysis
- [ ] Validate or independently recompute log2FC from protein abundances
- [ ] Apply DEqMS (limma-based, peptide-count-aware variance estimation) — or limma if peptide counts unavailable
- [ ] Define contrast: DRG vs SG
- [ ] Set significance thresholds (adjusted p-value < 0.05, |log2FC| > 1.0)
- [ ] Generate volcano plot, MA plot
- [ ] Classify proteins: DRG-enriched, SG-enriched, shared/unchanged
- [ ] Create ranked protein lists for downstream enrichment
- [ ] Sensitivity analysis: compare our DE results with core-provided log2FC
- **Status:** pending
- **Key decisions:** FC threshold, FDR method, whether to use core's stats or our own
- **Deliverables:** DE results table, volcano/MA plots, ranked protein lists

### Phase 5: Functional Enrichment & Pathway Analysis
- [ ] Gene Ontology enrichment (BP, MF, CC) on DRG-enriched and SG-enriched protein sets
- [ ] KEGG and Reactome pathway enrichment analysis
- [ ] Gene Set Enrichment Analysis (GSEA) using ranked log2FC list (threshold-free)
- [ ] Tissue-specific marker validation (known DRG markers: TRPV1, Nav1.7/SCN9A, CGRP; known SG markers: TH, DBH, NET)
- [ ] Ion channel and receptor sub-analysis (particularly relevant for pain/autonomic biology)
- [ ] Transcription factor enrichment (if sufficient coverage)
- **Status:** pending
- **Key decisions:** Enrichment tool (g:Profiler, clusterProfiler, or GSEApy), background set definition
- **Deliverables:** Enrichment tables, dot plots, GSEA enrichment plots, curated pathway figures

### Phase 6: Protein Interaction Network & Integration Analysis
- [ ] Build PPI network from DE proteins using STRING database
- [ ] Identify network hubs and modules (MCL clustering or MCODE)
- [ ] Cross-reference with existing human DRG/SG transcriptomic data (Tavares-Ferreira et al., Nguyen et al.)
- [ ] Proteome-transcriptome correlation analysis (where public data is available)
- [ ] Cell-type deconvolution or marker-based inference (neuronal vs glial vs immune vs vascular)
- [ ] Compare findings with the 2018 Barry et al. human DRG proteome
- **Status:** pending
- **Key decisions:** Network confidence threshold, transcriptomic datasets for integration
- **Deliverables:** Network figures, proteome-transcriptome comparison, cell-type composition estimates

### Phase 7: Visualization & Publication Figures
- [ ] Figure 1: Experimental workflow schematic
- [ ] Figure 2: QC panel (PCA, correlation heatmap, CV distribution, coverage stats)
- [ ] Figure 3: Volcano plot with key proteins labeled + protein counts summary
- [ ] Figure 4: Heatmap of top DE proteins (clustered, annotated)
- [ ] Figure 5: GO/pathway enrichment comparison (DRG vs SG side-by-side)
- [ ] Figure 6: PPI network of tissue-specific modules
- [ ] Figure 7: Focused analysis — ion channels, receptors, or pain-relevant proteins
- [ ] Supplementary figures: additional QC, full enrichment results, individual protein plots
- [ ] All figures as PNG (300 dpi) and SVG per project conventions
- **Status:** pending
- **Key decisions:** Figure layout, color scheme, journal format
- **Deliverables:** 7 main figures + supplementary, saved to `figures/`

### Phase 8: Report & Publication Drafting
- [ ] Write analysis report (methods, results, interpretation) for lab use
- [ ] Draft manuscript sections: Abstract, Introduction, Methods, Results, Discussion
- [ ] Compile supplementary tables (full protein list, DE results, enrichment results)
- [ ] Format references
- [ ] Export final results to `results/` directory
- [ ] Archive reproducible analysis pipeline with parameters documented
- **Status:** pending
- **Key decisions:** Word limits, emphasis (pain biology vs autonomic vs comparative anatomy)
- **Deliverables:** Analysis report, manuscript draft, supplementary materials, archived pipeline

---

## Key Questions
1. What is the tissue source? — **Local biorepository, organ donors**
2. Which mass spec instrument will be used? — **Bruker timsTOF (dia-PASEF), core facility**
3. What gradient length for LC? — **Core facility decision (document in methods)**
4. Target journal for publication? — **Journal of Proteomics**
5. Should phosphoproteomics or PTM analysis be included? — **TBD**
6. What exactly does the core return? — **Raw .d files, spectral counts, log2FC (confirm exact deliverables)**
7. Does the core provide peptide-level data (needed for DEqMS)? — **TBD — request this**

## Decisions Made
| Decision | Rationale |
|----------|-----------|
| dia-PASEF on timsTOF | Better quantitative reproducibility and coverage than DDA; ion mobility adds selectivity and depth |
| 4 biological replicates per tissue | Sufficient statistical power for DE analysis; standard for proteomics |
| Single-batch processing | Eliminates batch effects as a confounder |
| Core facility handles sample prep + acquisition | Standard practice; leverages core expertise and instrumentation |
| Independent re-analysis of core data | Core provides initial processing, but we validate and run our own statistical pipeline for rigor |
| Python-based pipeline | User preference; compatible with pandas, scipy, matplotlib, seaborn; R bridge via rpy2 for DEqMS if needed |
| DEqMS for differential expression | Proteomics-specific variance estimation (peptide-count aware); built on proven limma framework |

## Errors Encountered
| Error | Attempt | Resolution |
|-------|---------|------------|
| (none yet) | — | — |

## Notes
- All raw data goes in `data/raw/`, processed matrices in `data/processed/`
- Core facility outputs go in `data/processed/core_output/` to distinguish from our processing
- Figures saved as PNG (300 dpi) + SVG per CLAUDE.md conventions
- Analysis scripts in `src/analysis/`, utilities in `src/utils/`
- DEqMS is an R package — will need rpy2 bridge or a standalone R script called from Python
- **Important:** Request peptide-level quantification from core (not just protein-level) — needed for DEqMS
- **Important:** Document core's exact software versions and parameters for Methods section
