---
name: bcftools
description: >
  Call, filter, normalize, annotate, merge, and summarize genetic variants in
  VCF/BCF using bcftools (HTSlib). Use for common VCF/BCF tasks: indexing,
  subsetting by region/samples, filtering by INFO/FORMAT/expression, merging and
  concatenation, normalization (left-align/split multiallelics), annotation, and
  QC summary stats. Triggers on: .vcf, .vcf.gz, .bcf, "variant filtering",
  "normalize VCF", "bcftools view/filter/query/stats/norm/annotate/merge/concat".
---

# bcftools

## Environment

Binary: `bin/bcftools` — relative to this skill directory.

Before issuing any commands, resolve the full absolute path for this machine:
```bash
readlink -f "$(dirname <path-to-this-SKILL.md>)/bin/bcftools"
```
Substitute `<path-to-this-SKILL.md>` with the absolute path you used to read this file.
Use the printed output literally as the first token in every command.
In examples below, `$BCFTOOLS` is a readable placeholder for that resolved path.

## Subcommands

**Core viewing and filtering**
- `view` — subset and filter VCF/BCF (samples, regions, types, sites, GT, etc.)
- `filter` — filter records by expression; can set FILTER tags
- `query` — extract fields into tabular output with a format string

**Normalization and transforms**
- `norm` — normalize variants (split multiallelics, left-align, check REF)
- `annotate` — add/replace annotations (IDs, INFO/FORMAT) or rename contigs
- `reheader` — replace header (sample names, contigs) without rewriting records

**Merging, concatenation, and set ops**
- `merge` — merge VCF/BCF across samples
- `concat` — concatenate VCF/BCF (typically across genomic chunks)
- `isec` — intersections/differences between callsets

**Indexing and utilities**
- `index` — create index for bgzipped VCF or BCF
- `sort` — sort VCF/BCF records

**Statistics**
- `stats` — summary statistics; common entrypoint for QC

## Common patterns

**Index a bgzipped VCF (required for region queries):**
```bash
$BCFTOOLS index -t input.vcf.gz
```

**Subset by region and/or samples:**
```bash
# Region (requires index):
$BCFTOOLS view -r chr1:100000-200000 input.vcf.gz -Oz -o region.vcf.gz

# Samples by file (one sample per line):
$BCFTOOLS view -S samples.txt input.vcf.gz -Oz -o subset.samples.vcf.gz
```

**Keep only PASS variants (or drop filtered):**
```bash
$BCFTOOLS view -f PASS input.vcf.gz -Oz -o pass.vcf.gz
```

**Basic expression filtering (QUAL/DP/AF etc.):**
```bash
# Example: keep SNPs with QUAL>=30 and INFO/DP>=10
$BCFTOOLS view -v snps input.vcf.gz \
  | $BCFTOOLS filter -i 'QUAL>=30 && INFO/DP>=10' -Oz -o snps.q30.dp10.vcf.gz
```

**Normalize: split multiallelics + left-align (requires reference FASTA):**
```bash
$BCFTOOLS norm -m -any -f reference.fa input.vcf.gz -Oz -o normalized.vcf.gz
```

**Extract a table with `query` (CHROM, POS, REF, ALT, sample GT/DP):**
```bash
$BCFTOOLS query -f '%CHROM\t%POS\t%REF\t%ALT[\t%GT\t%DP]\n' input.vcf.gz > variants.tsv
```

**Merge two callsets across samples (same sites / compatible representation):**
```bash
$BCFTOOLS merge a.vcf.gz b.vcf.gz -Oz -o merged.vcf.gz
$BCFTOOLS index -t merged.vcf.gz
```

**Concatenate chromosome chunks (same samples; non-overlapping regions):**
```bash
$BCFTOOLS concat chr1.vcf.gz chr2.vcf.gz -Oz -o genome.concat.vcf.gz
$BCFTOOLS index -t genome.concat.vcf.gz
```

**QC stats quick summary:**
```bash
$BCFTOOLS stats input.vcf.gz | grep -E '^SN' | cut -f2-
```

## Allowlist entries

Resolve and add to your terminal command allowlist (Cursor: Settings → Features → Terminal):
```bash
readlink -f "$(dirname <path-to-this-SKILL.md>)/bin/bcftools"
```

## Full flag reference

To look up all flags for a specific subcommand:
```bash
grep -A 80 "^### \\`subcommand\\`" "$(dirname <path-to-this-SKILL.md>)/reference.md"
```
Full reference: [reference.md](reference.md)

## Patterns

Reusable real-world patterns accumulated over time. To search:
```bash
grep -A 20 "keyword" "$(dirname <path-to-this-SKILL.md>)/patterns.md"
```
[patterns.md](patterns.md)
