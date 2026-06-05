---
name: bedtools
description: >
  Perform genome arithmetic on BED/GFF/VCF/BAM interval files using bedtools.
  Use for overlapping intervals (intersect, window, closest), merging/sorting
  intervals, coverage and genome-wide coverage (coverage, genomecov, map),
  set operations (subtract, complement, merge, cluster), FASTA extraction
  (getfasta), format conversion (bamtobed, bamtofastq), and interval
  statistics (jaccard, fisher). Triggers on: .bed, .bed.gz, BED12, bedGraph,
  "interval overlap", "peak intersection", "coverage over regions", bedtools
  intersect/merge/subtract/genomecov/closest/getfasta.
---

# bedtools

## Environment

Binary: `bin/bedtools` — relative to this skill directory.

Before issuing any commands, resolve the full absolute path for this machine:
```bash
readlink -f "$(dirname <path-to-this-SKILL.md>)/bin/bedtools"
```
Substitute `<path-to-this-SKILL.md>` with the absolute path you used to read this file.
Use the printed output literally as the first token in every command.
In examples below, `$BEDTOOLS` is a readable placeholder for that resolved path.

## Subcommands

**Genome arithmetic**
- `intersect` — find overlapping intervals between two files (`-wa`, `-wb`, `-wo`, `-u`, `-v`, `-c`)
- `window` — report features in B within a window around each feature in A
- `closest` — nearest feature in B to each feature in A (optionally with distance)
- `coverage` — how much of each A interval is covered by B
- `map` — apply a summary function to B columns overlapping each A interval
- `genomecov` — genome-wide coverage histogram or per-base depth from BED or BAM
- `merge` — merge overlapping or nearby intervals
- `cluster` — cluster overlapping intervals (report cluster id, do not merge)
- `complement` — intervals in a genome not covered by an input file
- `shift` — shift interval coordinates
- `subtract` — remove intervals in B from A
- `slop` — expand interval boundaries
- `flank` — create flanking intervals
- `sort` — sort intervals (required before many operations)
- `random` / `shuffle` / `sample` — random or shuffled intervals
- `spacing` — report gaps between consecutive intervals
- `annotate` — annotate A with coverage counts from multiple B files

**Multi-way comparisons**
- `multiinter` — intervals common to multiple files
- `unionbedg` — combine multiple bedGraph files

**Paired-end**
- `pairtobed` / `pairtopair` — paired-end overlap tests

**Format conversion**
- `bamtobed` / `bedtobam` / `bamtofastq` / `bedpetobam` / `bed12tobed6`

**FASTA**
- `getfasta` — extract sequences for BED intervals from FASTA
- `maskfasta` / `nuc` — mask or profile nucleotide content

**BAM**
- `multicov` — coverage from multiple BAMs at BED intervals
- `tag` — tag BAM alignments by BED overlap

**Statistics**
- `jaccard` / `reldist` / `fisher` — interval set statistics

**Miscellaneous**
- `overlap` / `makewindows` / `groupby` / `expand` / `split` / `summary` / `igv` / `links`

## Common patterns

**Sort BED (required before merge, subtract, and sorted intersect):**
```bash
sort -k1,1 -k2,2n regions.bed > regions.sorted.bed
```

**Intersect peaks with genes (keep A intervals that overlap B):**
```bash
$BEDTOOLS intersect -a peaks.bed -b genes.bed -wa -u > peaks.with_gene_overlap.bed
```

**Count overlaps per peak:**
```bash
$BEDTOOLS intersect -a peaks.bed -b genes.bed -c > peaks.overlap_counts.bed
```

**Reciprocal overlap (≥50% of both intervals):**
```bash
$BEDTOOLS intersect -a a.bed -b b.bed -f 0.5 -r -wa -wb > a.b.reciprocal.bed
```

**Sorted intersect (large files; inputs must be sorted):**
```bash
$BEDTOOLS intersect -a a.sorted.bed -b b.sorted.bed -sorted -wa > a.hits.bed
```

**Subtract blacklist from peaks:**
```bash
$BEDTOOLS subtract -a peaks.sorted.bed -b blacklist.sorted.bed > peaks.filtered.bed
```

**Merge overlapping peaks:**
```bash
$BEDTOOLS merge -i peaks.sorted.bed > peaks.merged.bed
```

**Coverage of BED intervals:**
```bash
$BEDTOOLS coverage -a targets.bed -b alignments.bed > targets.coverage.bed
```

**Genome-wide coverage from BAM (genome file from `samtools faidx`):**
```bash
$BEDTOOLS genomecov -ibam alignments.sorted.bam -g genome.fa.fai -bg > coverage.bedgraph
```

**Closest feature with distance:**
```bash
$BEDTOOLS closest -a query.bed -b annotations.bed -d > query.nearest.bed
```

**Extract FASTA sequences for intervals:**
```bash
$BEDTOOLS getfasta -fi reference.fa -bed regions.bed -fo regions.fa
```

## Piping with other tools

BED files pipe through standard Unix tools; bedtools reads stdin when `-` is used:
```bash
awk '$3-$2 >= 100' peaks.bed | $BEDTOOLS sort | $BEDTOOLS merge -i - > peaks.wide.merged.bed
```

With samtools (BAM → BED, then interval ops) — resolve each skill’s `bin/<tool>` with `readlink -f`:
```bash
<SAMTOOLS> view -b -F 4 alignments.bam | <BEDTOOLS> bamtobed -i stdin > mapped.bed
```

## Allowlist entries

Resolve and add to your terminal command allowlist (Cursor: Settings → Features → Terminal):
```bash
readlink -f "$(dirname <path-to-this-SKILL.md>)/bin/bedtools"
```

## Full flag reference

To look up all flags for a specific subcommand:
```bash
grep -A 80 "^### \`subcommand\`" "$(dirname <path-to-this-SKILL.md>)/reference.md"
```
Full reference: [reference.md](reference.md)

## Patterns

Reusable real-world patterns accumulated over time. To search:
```bash
grep -A 20 "keyword" "$(dirname <path-to-this-SKILL.md>)/patterns.md"
```
[patterns.md](patterns.md)
