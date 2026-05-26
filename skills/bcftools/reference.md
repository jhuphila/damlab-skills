# bcftools — Full Reference

Binary: `bin/bcftools` (relative to this skill directory)

Each entry contains the verbatim `--help` output. Grep for a subcommand:
```bash
grep -A 80 "^### \`subcommand\`" "$(dirname <path-to-this-SKILL.md>)/reference.md"
```
Increase `-A` if output appears truncated.

---

## Global

### `bcftools`

```
Program: bcftools (Tools for variant calling and manipulating VCFs and BCFs)
License: GNU GPLv3+, due to use of the GNU Scientific Library
Version: 1.23.1 (using htslib 1.23.1)

Usage:   bcftools [--version|--version-only] [--help] <command> <argument>

Commands:

 -- Indexing
    index        index VCF/BCF files

 -- VCF/BCF manipulation
    annotate     annotate and edit VCF/BCF files
    concat       concatenate VCF/BCF files from the same set of samples
    convert      convert VCF/BCF files to different formats and back
    head         view VCF/BCF file headers
    isec         intersections of VCF/BCF files
    merge        merge VCF/BCF files files from non-overlapping sample sets
    norm         left-align and normalize indels
    plugin       user-defined plugins
    query        transform VCF/BCF into user-defined formats
    reheader     modify VCF/BCF header, change sample names
    sort         sort VCF/BCF file
    view         VCF/BCF conversion, view, subset and filter VCF/BCF files

 -- VCF/BCF analysis
    call         SNP/indel calling
    consensus    create consensus sequence by applying VCF variants
    cnv          HMM CNV calling
    csq          call variation consequences
    filter       filter VCF/BCF files using fixed thresholds
    gtcheck      check sample concordance, detect sample swaps and contamination
    mpileup      multi-way pileup producing genotype likelihoods
    polysomy     detect number of chromosomal copies
    roh          identify runs of autozygosity (HMM)
    stats        produce VCF/BCF stats

 -- Plugins (collection of programs for calling, file manipulation & analysis)
    41 plugins available, run "bcftools plugin -lv" to see a complete list

 Most commands accept VCF, bgzipped VCF, and BCF with the file type detected
 automatically even when streaming from a pipe. Indexed VCF and BCF will work
 in all situations. Un-indexed VCF and BCF and streams will work in most but
 not all situations.
```

## Viewing and filtering

### `view`

```
view: unrecognized option '--help'

About:   VCF/BCF conversion, view, subset and filter VCF/BCF files.
Usage:   bcftools view [options] <in.vcf.gz> [region1 [...]]

Output options:
    -G, --drop-genotypes              Drop individual genotype information (after subsetting if -s option set)
    -h, --header-only                 Print only the header in VCF output (equivalent to bcftools head)
    -H, --no-header                   Suppress the header in VCF output
        --with-header                 Print both header and records in VCF output [default]
    -l, --compression-level [0-9]     Compression level: 0 uncompressed, 1 best speed, 9 best compression [-1]
        --no-version                  Do not append version and command line to the header
    -o, --output FILE                 Output file name [stdout]
    -O, --output-type u|b|v|z[0-9]    u/b: un/compressed BCF, v/z: un/compressed VCF, 0-9: compression level [v]
    -r, --regions REGION              Restrict to comma-separated list of regions
    -R, --regions-file FILE           Restrict to regions listed in FILE
        --regions-overlap 0|1|2       Include if POS in the region (0), record overlaps (1), variant overlaps (2) [1]
    -t, --targets [^]REGION           Similar to -r but streams rather than index-jumps. Exclude regions with "^" prefix
    -T, --targets-file [^]FILE        Similar to -R but streams rather than index-jumps. Exclude regions with "^" prefix
        --targets-overlap 0|1|2       Include if POS in the region (0), record overlaps (1), variant overlaps (2) [0]
        --threads INT                 Use multithreading with INT worker threads [0]
        --verbosity INT               Verbosity level

Subset options:
    -A, --trim-unseen-allele          Remove '<*>' or '<NON_REF>' at variant (-A) or at all (-AA) sites
    -a, --trim-alt-alleles            Trim ALT alleles not seen in the genotype fields (or their subset with -s/-S)
    -I, --no-update                   Do not (re)calculate INFO fields for the subset (currently INFO/AC and INFO/AN)
    -s, --samples [^]LIST             Comma separated list of samples to include (or exclude with "^" prefix). Be careful
                                        when combining filtering with sample subsetting as filtering comes (usually) first.
                                        If unsure, split sample subsetting and filtering in two commands, using -Ou when piping.
    -S, --samples-file [^]FILE        File of samples to include (or exclude with "^" prefix)
        --force-samples               Only warn about unknown subset samples

Filter options:
    -c/C, --min-ac/--max-ac INT[:TYPE]     Minimum/maximum count for non-reference (nref), 1st alternate (alt1), least frequent
                                               (minor), most frequent (major) or sum of all but most frequent (nonmajor) alleles [nref]
    -f,   --apply-filters LIST             Require at least one of the listed FILTER strings (e.g. "PASS,.")
    -g,   --genotype [^]hom|het|miss       Require one or more hom/het/missing genotype or, if prefixed with "^", exclude such sites
    -i/e, --include/--exclude EXPR         Select/exclude sites for which the expression is true (see man page for details)
    -k/n, --known/--novel                  Select known/novel sites only (ID is not/is '.')
    -m/M, --min-alleles/--max-alleles INT  Minimum/maximum number of alleles listed in REF and ALT (e.g. -m2 -M2 for biallelic sites)
    -p/P, --phased/--exclude-phased        Select/exclude sites where all samples are phased
    -q/Q, --min-af/--max-af FLOAT[:TYPE]   Minimum/maximum frequency for non-reference (nref), 1st alternate (alt1), least frequent
                                               (minor), most frequent (major) or sum of all but most frequent (nonmajor) alleles [nref]
    -u/U, --uncalled/--exclude-uncalled    Select/exclude sites without a called genotype
    -v/V, --types/--exclude-types LIST     Select/exclude comma-separated list of variant types: snps,indels,mnps,ref,bnd,other [null]
    -x/X, --private/--exclude-private      Select/exclude sites where the non-reference alleles are exclusive (private) to the subset samples
    -W,   --write-index[=FMT]              Automatically index the output files [off]
```

### `filter`

```
filter: unrecognized option '--help'

About:   Apply fixed-threshold filters.
Usage:   bcftools filter [options] <in.vcf.gz>

Options:
    -e, --exclude EXPR             Exclude sites for which the expression is true (see man page for details)
    -g, --SnpGap INT[:TYPE]        Filter SNPs within <int> base pairs of an indel (the default) or any combination of indel,mnp,bnd,other,overlap
    -G, --IndelGap INT             Filter clusters of indels separated by <int> or fewer base pairs allowing only one to pass
    -i, --include EXPR             Include only sites for which the expression is true (see man page for details
        --mask [^]REGION           Soft filter regions, "^" to negate
    -M, --mask-file [^]FILE        Soft filter regions listed in a file, "^" to negate
        --mask-overlap 0|1|2       Mask if POS in the region (0), record overlaps (1), variant overlaps (2) [1]
    -m, --mode [+x]                "+": do not replace but add to existing FILTER; "x": reset filters at sites which pass
        --no-version               Do not append version and command line to the header
    -o, --output FILE              Write output to a file [standard output]
    -O, --output-type u|b|v|z[0-9] u/b: un/compressed BCF, v/z: un/compressed VCF, 0-9: compression level [v]
    -r, --regions REGION           Restrict to comma-separated list of regions
    -R, --regions-file FILE        Restrict to regions listed in a file
        --regions-overlap 0|1|2    Include if POS in the region (0), record overlaps (1), variant overlaps (2) [1]
    -s, --soft-filter STRING       Annotate FILTER column with <string> or unique filter name ("Filter%d") made up by the program ("+")
    -S, --set-GTs .|0              Set genotypes of failed samples to missing (.) or ref (0)
    -t, --targets REGION           Similar to -r but streams rather than index-jumps
    -T, --targets-file FILE        Similar to -R but streams rather than index-jumps
        --targets-overlap 0|1|2    Include if POS in the region (0), record overlaps (1), variant overlaps (2) [0]
        --threads INT              Use multithreading with <int> worker threads [0]
    -v, --verbosity INT            Verbosity level
    -W, --write-index[=FMT]        Automatically index the output files [off]
```

### `query`

```
About:   Extracts fields from VCF/BCF file and prints them in user-defined format
Usage:   bcftools query [options] <A.vcf.gz> [<B.vcf.gz> [...]]

Options:
    -e, --exclude EXPR                Exclude sites for which the expression is true (see man page for details)
        --force-samples               Only warn about unknown subset samples
    -F, --print-filtered STR          Output STR for samples failing the -i/-e filtering expression
    -f, --format STRING               See man page for details
    -H, --print-header                Print header, -HH to omit column indices
    -i, --include EXPR                Select sites for which the expression is true (see man page for details)
    -l, --list-samples                Print the list of samples and exit
    -N, --disable-automatic-newline   Disable automatic addition of newline character when not present
    -o, --output FILE                 Output file name [stdout]
    -r, --regions REGION              Restrict to comma-separated list of regions
    -R, --regions-file FILE           Restrict to regions listed in a file
        --regions-overlap 0|1|2       Include if POS in the region (0), record overlaps (1), variant overlaps (2) [1]
    -s, --samples LIST                List of samples to include
    -S, --samples-file FILE           File of samples to include
    -t, --targets REGION              Similar to -r but streams rather than index-jumps
    -T, --targets-file FILE           Similar to -R but streams rather than index-jumps
        --targets-overlap 0|1|2       Include if POS in the region (0), record overlaps (1), variant overlaps (2) [0]
    -u, --allow-undef-tags            Print "." for undefined tags
    -v, --vcf-list FILE               Process multiple VCFs listed in the file
        --verbosity INT               Verbosity level

Examples:
	bcftools query -f '%CHROM\t%POS\t%REF\t%ALT[\t%SAMPLE=%GT]\n' file.vcf.gz
	# For more examples see http://samtools.github.io/bcftools/bcftools.html#query
```

## Normalization and transforms

### `norm`

```
About:   Left-align and normalize indels; check if REF alleles match the reference;
         split multiallelic sites into multiple rows; recover multiallelics from
         multiple rows.
Usage:   bcftools norm [options] <in.vcf.gz>

Options:
    -a, --atomize                   Decompose complex variants (e.g. MNVs become consecutive SNVs)
        --atom-overlaps '*'|.       Use the star allele (*) for overlapping alleles or set to missing (.) [*]
    -c, --check-ref e|w|x|s         Check REF alleles and exit (e), warn (w), exclude (x), or set (s) bad sites [e]
    -D, --remove-duplicates         Remove duplicate lines of the same type.
    -d, --rm-dup TYPE               Remove duplicate snps|indels|both|all|exact
    -e, --exclude EXPR              Do not normalize records for which the expression is true (see man page for details)
    -f, --fasta-ref FILE            Reference sequence
        --force                     Try to proceed even if malformed tags are encountered. Experimental, use at your own risk
    -g, --gff-annot FILE            Follow HGVS 3'rule and right-align variants in transcripts on the forward strand
    -i, --include EXPR              Normalize only records for which the expression is true (see man page for details)
        --keep-sum TAG,..           Keep vector sum constant when splitting multiallelics (see github issue #360)
    -m, --multiallelics -|+TYPE     Split multiallelics (-) or join biallelics (+), type: snps|indels|both|any [both]
        --multi-overlaps 0|.        Fill in the reference (0) or missing (.) allele when splitting multiallelics [0]
        --no-version                Do not append version and command line to the header
    -N, --do-not-normalize          Do not normalize indels (with -m or -c s)
        --old-rec-tag STR           Annotate modified records with INFO/STR indicating the original variant
    -o, --output FILE               Write output to a file [standard output]
    -O, --output-type u|b|v|z[0-9]  u/b: un/compressed BCF, v/z: un/compressed VCF, 0-9: compression level [v]
    -r, --regions REGION            Restrict to comma-separated list of regions
    -R, --regions-file FILE         Restrict to regions listed in a file
        --regions-overlap 0|1|2     Include if POS in the region (0), record overlaps (1), variant overlaps (2) [1]
    -s, --strict-filter             When merging (-m+), merged site is PASS only if all sites being merged PASS
    -S, --sort METHOD               Sort order: chr_pos,lex [chr_pos]
    -t, --targets REGION            Similar to -r but streams rather than index-jumps
    -T, --targets-file FILE         Similar to -R but streams rather than index-jumps
        --targets-overlap 0|1|2     Include if POS in the region (0), record overlaps (1), variant overlaps (2) [0]
        --threads INT               Use multithreading with INT worker threads [0]
    -v, --verbosity INT             Verbosity level
    -w, --site-win INT              Buffer for sorting lines which changed position during realignment [1000]
    -W, --write-index[=FMT]         Automatically index the output files [off]

Examples:
   # normalize and left-align indels
   bcftools norm -f ref.fa in.vcf

   # split multi-allelic sites
   bcftools norm -m- in.vcf
```

### `annotate`

```
annotate: unrecognized option '--help'

About:   Annotate and edit VCF/BCF files.
Usage:   bcftools annotate [options] VCF

Options:
   -a, --annotations FILE          VCF file or tabix-indexed FILE with annotations: CHR\tPOS[\tVALUE]+
   -c, --columns LIST              List of columns in the annotation file, e.g. CHROM,POS,REF,ALT,-,INFO/TAG. See man page for details
   -C, --columns-file FILE         Read -c columns from FILE, one name per row, with optional --merge-logic TYPE: NAME[ TYPE]
   -e, --exclude EXPR              Exclude sites for which the expression is true (see man page for details)
       --force                     Continue despite parsing error (at your own risk!)
   -H, --header-line STR           Header line which should be appended to the VCF header, can be given multiple times
   -h, --header-lines FILE         Lines which should be appended to the VCF header
   -I, --set-id [+]FORMAT          Set ID column using a `bcftools query`-like expression, see man page for details
   -i, --include EXPR              Select sites for which the expression is true (see man page for details)
   -k, --keep-sites                Leave -i/-e sites unchanged instead of discarding them
   -l, --merge-logic TAG:TYPE      Merge logic for multiple overlapping regions (see man page for details), EXPERIMENTAL
   -m, --mark-sites [+-]TAG        Add INFO/TAG flag to sites which are ("+") or are not ("-") listed in the -a file
       --min-overlap ANN:VCF       Required overlap as a fraction of variant in the -a file (ANN), the VCF (:VCF), or reciprocal (ANN:VCF)
       --no-version                Do not append version and command line to the header
   -o, --output FILE               Write output to a file [standard output]
   -O, --output-type u|b|v|z[0-9]  u/b: uncompressed BCF, v/z: compressed VCF, 0-9: compression level [v]
       --pair-logic STR            Matching records by <snps|indels|both|all|some|exact|id>, see man page for details [some]
   -r, --regions REGION            Restrict to comma-separated list of regions
   -R, --regions-file FILE         Restrict to regions listed in FILE
       --regions-overlap 0|1|2     Include if POS in the region (0), record overlaps (1), variant overlaps (2) [1]
       --rename-annots FILE        Rename annotations: TYPE/old\tnew, where TYPE is one of FILTER,INFO,FORMAT
       --rename-chrs FILE          Rename sequences according to the mapping: old\tnew
   -s, --samples [^]LIST           Comma separated list of samples to annotate (or exclude with "^" prefix)
   -S, --samples-file [^]FILE      File of samples to annotate (or exclude with "^" prefix)
       --single-overlaps           Keep memory low by avoiding complexities arising from handling multiple overlapping intervals
   -x, --remove LIST               List of annotations (e.g. ID,INFO/DP,FORMAT/DP,FILTER) to remove (or keep with "^" prefix). See man page for details
       --threads INT               Number of extra output compression threads [0]
   -v, --verbosity INT             Verbosity level
   -W, --write-index[=FMT]         Automatically index the output files [off]

Examples:
   http://samtools.github.io/bcftools/howtos/annotate.html
```

### `reheader`

```
reheader: unrecognized option '--help'

About:   Modify header of VCF/BCF files, change sample names.
Usage:   bcftools reheader [OPTIONS] <in.vcf.gz>

Options:
    -f, --fai FILE             Update sequences and their lengths from the .fai file
    -h, --header FILE          New header
    -o, --output FILE          Write output to a file [standard output]
    -n, --samples-list LIST    New sample names given as a comma-separated list
    -N, --samples-file FILE    New sample names in a file, see the man page for details
    -T, --temp-prefix PATH     Ignored; was template for temporary file name
        --threads INT          Use multithreading with INT worker threads (BCF only) [0]
    -v, --verbosity INT        Verbosity level

Example:
   # Write out the header to be modified
   bcftools view -h old.bcf > header.txt

   # Edit the header using your favorite text editor
   vi header.txt

   # Reheader the file
   bcftools reheader -h header.txt -o new.bcf old.bcf
```

## Merging, concatenation, and set ops

### `merge`

```
About:   Merge multiple VCF/BCF files from non-overlapping sample sets to create one multi-sample file.
         Note that only records from different files can be merged, never from the same file. For
         "vertical" merge take a look at "bcftools norm" instead.
Usage:   bcftools merge [options] <A.vcf.gz> <B.vcf.gz> [...]

Options:
        --force-no-index              Merge unindexed files, synonymous to --no-index
        --force-samples               Resolve duplicate sample names
        --force-single                Run even if there is only one file on input
        --print-header                Print only the merged header and exit
        --use-header FILE             Use the provided header
    -0  --missing-to-ref              Assume genotypes at missing sites are 0/0
    -f, --apply-filters LIST          Require at least one of the listed FILTER strings (e.g. "PASS,.")
    -F, --filter-logic x|+            Remove filters if some input is PASS ("x"), or apply all filters ("+") [+]
    -g, --gvcf -|REF.FA               Merge gVCF blocks, INFO/END tag is expected. Implies -i QS:sum,MinDP:min,MIN_DP:min,I16:sum,IDV:max,IMF:max -M PL:max,AD:0
    -i, --info-rules TAG:METHOD,..    Rules for merging INFO fields (method is one of sum,avg,min,max,join) or "-" to turn off the default [DP:sum,DP4:sum]
    -l, --file-list FILE              Read file names from the file
    -L, --local-alleles INT           If more than INT alt alleles are encountered, drop FMT/PL and output LAA+LPL instead; 0=unlimited [0]
    -m, --merge STRING[*|**]          Allow multiallelic records for snps,indels,both,snp-ins-del,all,none,id,*,**; see man page for details [both]
    -M, --missing-rules TAG:METHOD    Rules for replacing missing values in numeric vectors (.,0,max) when unknown allele <*> is not present [.]
        --no-index                    Merge unindexed files, the same chromosomal order is required and -r/-R are not allowed
        --no-version                  Do not append version and command line to the header
    -o, --output FILE                 Write output to a file [standard output]
    -O, --output-type u|b|v|z[0-9]    u/b: uncompressed BCF, v/z: compressed VCF, 0-9: compression level [v]
    -r, --regions REGION              Restrict to comma-separated list of regions
    -R, --regions-file FILE           Restrict to regions listed in a file
        --regions-overlap 0|1|2       Include if POS in the region (0), record overlaps (1), variant overlaps (2) [1]
        --threads INT                 Use multithreading with INT worker threads [0]
    -v, --verbosity INT               Verbosity level
    -W, --write-index[=FMT]           Automatically index the output files [off]
```

### `concat`

```
concat: unrecognized option '--help'

About:   Concatenate or combine VCF/BCF files. All source files must have the same sample
         columns appearing in the same order. The program can be used, for example, to
         concatenate chromosome VCFs into one VCF, or combine a SNP VCF and an indel
         VCF into one. The input files must be sorted by chr and position. The files
         must be given in the correct order to produce sorted VCF on output unless
         the -a, --allow-overlaps option is specified. With the --naive option, the files
         are concatenated without being recompressed, which is very fast.
Usage:   bcftools concat [options] <A.vcf.gz> [<B.vcf.gz> [...]]

Options:
   -a, --allow-overlaps           First coordinate of the next file can precede last record of the current file.
   -c, --compact-PS               Do not output PS tag at each site, only at the start of a new phase set block.
   -d, --rm-dups STRING           Output duplicate records present in multiple files only once: <snps|indels|both|all|exact>
   -D, --remove-duplicates        Alias for -d exact
   -f, --file-list FILE           Read the list of files from a file.
   -G, --drop-genotypes           Drop individual genotype information.
   -l, --ligate                   Ligate phased VCFs by matching phase at overlapping haplotypes
       --ligate-force             Ligate even non-overlapping chunks, keep all sites
       --ligate-warn              Drop sites in imperfect overlaps
       --no-version               Do not append version and command line to the header
   -n, --naive                    Concatenate files without recompression, a header check compatibility is performed
       --naive-force              Same as --naive, but header compatibility is not checked. Dangerous, use with caution.
   -o, --output FILE              Write output to a file [standard output]
   -O, --output-type u|b|v|z[0-9] u/b: uncompressed BCF, v/z: compressed VCF, 0-9: compression level [v]
   -q, --min-PQ INT               Break phase set if phasing quality is lower than <int> [30]
   -r, --regions REGION           Restrict to comma-separated list of regions
   -R, --regions-file FILE        Restrict to regions listed in FILE
       --regions-overlap 0|1|2    Include if POS in the region (0), record overlaps (1), variant overlaps (2) [1]
       --threads INT              Use multithreading with <int> worker threads [0]
   -v, --verbosity INT            Set verbosity level
   -W, --write-index[=FMT]        Automatically index the output files [off]
```

### `isec`

```
About:   Create intersections, unions and complements of VCF files.
Usage:   bcftools isec [options] <A.vcf.gz> <B.vcf.gz> [...]

Options:
    -c, --collapse STRING          Treat as identical records with <snps|indels|both|all|some|none|id>, see man page for details [none]
    -C, --complement               Output positions present only in the first file but missing in the others
    -e, --exclude EXPR             Exclude sites for which the expression is true
    -f, --apply-filters LIST       Require at least one of the listed FILTER strings (e.g. "PASS,.")
    -i, --include EXPR             Include only sites for which the expression is true
    -l, --file-list FILE           Read the input file names from the file
        --no-version               Do not append version and command line to the header
    -n, --nfiles [+-=~]INT         Output positions present in this many (=), this many or more (+), this many or fewer (-), the exact (~) files
    -o, --output FILE              Write output to a file [standard output]
    -O, --output-type u|b|v|z[0-9] u/b: uncompressed BCF, v/z: compressed VCF, 0-9: compression level [v]
    -p, --prefix DIR               If given, subset each of the input files accordingly, see also -w
    -r, --regions REGION           Restrict to comma-separated list of regions
    -R, --regions-file FILE        Restrict to regions listed in a file
        --regions-overlap 0|1|2    Include if POS in the region (0), record overlaps (1), variant overlaps (2) [1]
    -t, --targets REGION           Similar to -r but streams rather than index-jumps
    -T, --targets-file FILE        Similar to -R but streams rather than index-jumps
        --targets-overlap 0|1|2    Include if POS in the region (0), record overlaps (1), variant overlaps (2) [0]
        --threads INT              Use multithreading with INT worker threads [0]
    -v, --verbosity INT            Verbosity level
    -w, --write LIST               List of files to write with -p given as 1-based indexes. By default, all files are written
    -W, --write-index[=FMT]        Automatically index the output files [off]

Examples:
   # Create intersection and complements of two sets saving the output in dir/*
   bcftools isec A.vcf.gz B.vcf.gz -p dir

   # Filter sites in A and B (but not in C) and create intersection
   bcftools isec -e'MAF<0.01' -i'dbSNP=1' -e - A.vcf.gz B.vcf.gz C.vcf.gz -p dir

   # Extract and write records from A shared by both A and B using exact allele match
   bcftools isec A.vcf.gz B.vcf.gz -p dir -n =2 -w 1

   # Extract and write records from C found in A and C but not in B
   bcftools isec A.vcf.gz B.vcf.gz C.vcf.gz -p dir -n~101 -w 3

   # Extract records private to A or B comparing by position only
   bcftools isec A.vcf.gz B.vcf.gz -p dir -n -1 -c all
```

## Indexing and utilities

### `index`

```
index: unrecognized option '--help'

About:   Index bgzip compressed VCF/BCF files for random access.
Usage:   bcftools index [options] <in.bcf>|<in.vcf.gz>

Indexing options:
    -c, --csi                generate CSI-format index for VCF/BCF files [default]
    -f, --force              overwrite index if it already exists
    -m, --min-shift INT      set minimal interval size for CSI indices to 2^INT [14]
    -o, --output FILE        optional output index file name
    -t, --tbi                generate TBI-format index for VCF files
        --threads INT        use multithreading with INT worker threads [0]
    -v, --verbosity INT      verbosity level

Stats options:
    -a, --all            with --stats, print stats for all contigs even when zero
    -n, --nrecords       print number of records based on existing index file
    -s, --stats          print per contig stats based on existing index file
```

### `sort`

```
About:   Sort VCF/BCF file.
Usage:   bcftools sort [OPTIONS] <FILE.vcf>

Options:
    -m, --max-mem FLOAT[kMG]       Maximum memory to use [768M]
    -o, --output FILE              Output file name [stdout]
    -O, --output-type u|b|v|z[0-9] u/b: uncompressed BCF, v/z: compressed VCF, 0-9: compression level [v]
    -T, --temp-dir DIR             Temporary files [/tmp/bcftools.XXXXXX]
    -v, --verbosity INT            Verbosity level
    -W, --write-index[=FMT]        Automatically index the output files [off]
```

## Statistics

### `stats`

```
About:   Parses VCF or BCF and produces stats which can be plotted using plot-vcfstats.
         When two files are given, the program generates separate stats for intersection
         and the complements. By default only sites are compared, -s/-S must given to include
         also sample columns.
Usage:   bcftools stats [options] <A.vcf.gz> [<B.vcf.gz>]

Options:
        --af-bins LIST               Allele frequency bins, a list (0.1,0.5,1) or a file (0.1\n0.5\n1)
        --af-tag STRING              Allele frequency tag to use, by default estimated from AN,AC or GT
    -1, --1st-allele-only            Include only 1st allele at multiallelic sites
    -c, --collapse STRING            Treat as identical records with <snps|indels|both|all|some|none>, see man page for details [none]
    -d, --depth INT,INT,INT          Depth distribution: min,max,bin size [0,500,1]
    -e, --exclude EXPR               Exclude sites for which the expression is true (see man page for details)
    -E, --exons FILE.gz              Tab-delimited file with exons for indel frameshifts (chr,beg,end; 1-based, inclusive, bgzip compressed)
    -f, --apply-filters LIST         Require at least one of the listed FILTER strings (e.g. "PASS,.")
    -F, --fasta-ref FILE             Faidx indexed reference sequence file to determine INDEL context
    -i, --include EXPR               Select sites for which the expression is true (see man page for details)
    -I, --split-by-ID                Collect stats for sites with ID separately (known vs novel)
    -r, --regions REGION             Restrict to comma-separated list of regions
    -R, --regions-file FILE          Restrict to regions listed in a file
        --regions-overlap 0|1|2      Include if POS in the region (0), record overlaps (1), variant overlaps (2) [1]
    -s, --samples LIST               List of samples for sample stats, "-" to include all samples
    -S, --samples-file FILE          File of samples to include
    -t, --targets REGION             Similar to -r but streams rather than index-jumps
    -T, --targets-file FILE          Similar to -R but streams rather than index-jumps
        --targets-overlap 0|1|2      Include if POS in the region (0), record overlaps (1), variant overlaps (2) [0]
    -u, --user-tstv TAG[:min:max:n]  Collect Ts/Tv stats for any tag using the given binning [0:1:100]
                                       A subfield can be selected as e.g. 'PV4[0]', here the first value of the PV4 tag
        --threads INT                Use multithreading with <int> worker threads [0]
    -v, --verbosity INT              Verbosity level
```

