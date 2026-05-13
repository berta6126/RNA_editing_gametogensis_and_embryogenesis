# RNA editing during gametogenesis and embryogenesis

This repository contains scripts and reproducible workflows for analyzing A-to-I RNA editing in bulk and single-cell RNA-seq data. It collects the code used in Eliad et al.,2026, along with documentation and environment files to allow reviewers and readers to reproduce the results.

## Overview

RNA editing by ADAR enzymes alters adenosines to inosines in RNA molecules. Understanding how editing patterns change across developmental stages helps reveal post-transcriptional regulation. Here, we analyzed bulk and single-cell RNA-seq data from gametes, embryos, and L4 tsgaed worms to identify and validate editing sites.

The analysis pipeline involves:

- Alignment of sequencing reads to the reference genome using STAR with optimized parameters to minimize mapping noise (`outFilterMultimapNmax=2` and `outFilterMismatchNmax=3` in End-to-End mode).
- Extraction and merging of candidate editing sites from multiple sources: known sites, Bowtie-derived sites (updated to the ce11 reference), and de novo sites from bulk RNA-seq.
- Reannotation of pileup files with gene annotations using `annotate_pileup_with_BED.py`.
- Merging editing calls by gene using `merge_sites_by_genes_in_pileup_new.py` and calculation of editing frequencies per gene.
- Verification and comparison of scripts for consistency (e.g., scripts contributed by collaborators).
- Calculation of mean read counts across biological replicates and preparation of the final editing tables.
- Downstream analyses such as motif extraction, free-energy comparisons, and heatmap visualizations (see `Analysis of editing sites – heatmaps` in the documentation notebook).

## Repository structure

```text
scripts/
  annotate_pileup_with_BED.py
  annotate_pileup_with_BED_for_pileups_with_chr.py
  convert_pileup_coordinates.py
  counting_reads_from_annotated_sam.py
  creating_an_editing_sites_list.py
  creating_bed_file_from_ncbi_gtf.py
  creating_bed_file_from_wormbase_gtf.py
  creating_bed_file_with_parts_from_wormbase_gff3.py
  editing_finder.py
  find_nucleotide_substitutions_in_at_least_2_samples.py
  finding_editing_site_at_least_x_samples.py
  free_energy_spliced_vs_unspliced.py
  get_list_of_genes_from_annotated_pileup.py
  get_seq_from_DNA.py
  merge_pileups.py
  merge_sites_by_genes_in_pileup.py
  merge_sites_by_genes_in_pileup_new.py
  merge_sites_by_genes_part_in_pileup.py
  motif_extraction.py
  pileup_filtration.py
  removing_VCF_data_from_pileup_new.py
  removing_negative_data_and_SNPs_from_pileup.py
  removing_negative_data_from_pileup.py
  split_intron_exon.py
  merging_counts.py
  merging_editing_calc_files.R
  merging_editing_summary_files.R
  merging_known_editing_sites_ce11.R
docs/
  Final_Analysis.md        # narrative extracted from the notebook (from “Final Analysis” to “Analysis of editing sites – heatmaps”)
environment.yml            # Conda environment file
.gitignore
```

Large genomic resources (.bed/.gff files, raw FASTQ/BAM files) are intentionally excluded; please download them separately as described in the paper.

## Reproducing the analysis

1. Install Conda (or Miniconda).
2. Create the environment:
   ```bash
   conda env create -f environment.yml
   conda activate rna_editing
   ```
3. Place the required input data (pileup files, annotation BED/GFF files) in a `data/` directory. See the manuscript for download links.
4. Run the scripts in the order described below. Examples:
   ```bash
   # Annotate pileups with gene information
   python scripts/annotate_pileup_with_BED.py <input_pileup> <BED_file> > <annotated_output>

   # Merge editing sites by gene
   python scripts/merge_sites_by_genes_in_pileup_new.py <annotated_pileup> > <gene_merged_output>

   # Calculate mean read counts across replicates
   python scripts/merging_counts.py
   ```
Detailed instructions for each step and parameter choices are recorded in the documentation notebook (`docs/Final_Analysis.md`) and the manuscript.

## References
Eliad et al., 2026 - If you use this code, please cite the accompanying paper.

---

Feel free to open issues or pull requests if you encounter problems or have suggestions.
