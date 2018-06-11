<br>
# Variant calling with Snippy

Keywords: variant calling, SNP, Snippy, JBrowse, Galaxy, Microbial Genomics Virtual Lab

## Background

Variant calling is the process of identifying differences between two genome samples.
Usually differences are limited to single nucleotide polymorphisms (SNPs) and small insertions and deletions (indels). Larger structural variation such as inversions, duplications and large deletions are not typically covered by "variant calling".

In this tutorial, we will use the tool "Snippy" (link to Snippy is [here](https://github.com/tseemann/snippy)). Snippy uses a tool to align the reads to a reference genome, and another tool to decide ("call") if the discrepancies are real variants.

## Learning Objectives

1. Find variants between a reference genome and a set of reads
2. Visualise the SNP in context of the reads aligned to the genome
3. Determine the effect of those variants on genomic features
4. Understand if the SNP is potentially affecting the phenotype

## Prepare reference

<!-- We will import  history from Galaxy:

- In the menu options across the top, go to <ss>Shared Data</ss>.
- Click on <ss>Histories</ss>.
- A list of published histories should appear. Click on <fn>GCC 2016 small genome variants</fn>.
- Click on <ss>Import history</ss>.
- An option will appear to re-name the history. We don't need to rename it, so click <ss>Import</ss>.
- The history will now appear in your Current History pane, and the files are now ready to use in Galaxy analyses.
-->


<!-- We will use the same data that we used in the [Assembly with Spades tutorial.](../spades/index.md) This should still be in your current galaxy history. If not, re-import the data into a new history using the instructions in that tutorial.-->

For variant calling, we need a reference genome that is of the same strain as the input sequence reads.

For this tutorial, our reference is the <fn>wildtype.gbk</fn> file and our reads are <fn>mutant_R1.fastq</fn> and <fn>mutant_R2.fastq</fn>.

If these files are not presently in your Galaxy history, import them from the [Training dataset page.](../data-dna/index.md)

## Call variants with Snippy

- Go to <ss>Tools &rarr; NGS Analysis &rarr; NGS: Variant Analysis &rarr; snippy</ss>
- For <ss>Reference type</ss> select *Genbank*.
- Then for <ss>Reference Genbank</ss> choose the <fn>wildtype.gbk</fn> file.
- For <ss>Single or Paired-end reads</ss> choose *Paired*.
- Then choose the first set of reads, <fn>mutant_R1.fastq</fn> and second set of reads, <fn>mutant_R2.fastq</fn>.
- For <ss>Cleanup the non-snp output files</ss> select *No*.

Your tool interface should look like this:

![Snippy interface](images/interface.png)

- Click <ss>Execute</ss>.

## Examine Snippy output

<!-- First, enable "Scratchbook" in Galaxy - this allows you to view several windows simultaneously. Click on the squares:

![scratchbook icon](images/scratchbook.png)

-->

From Snippy, there are 10 output files in various formats.

- Go to the file called <fn>snippy on data XX, data XX and data XX table</fn> and click on the eye icon.
- We can see a list of variants. Look in column 3 to see which types the variants are, such as a SNP or a deletion.
- Look at the third variant called. This is a T&rarr;A mutation, causing a stop codon. Look at column 14: the product of this gene is a methicillin resistance protein. Methicillin is an antibiotic. What might be the result of such a mutation? <!--[add a hint/info box]-->

## View Snippy output in JBrowse

- Go to <ss>Statistics and Visualisation &rarr; Graph/Display Data &rarr; JBrowse genome browser</ss>.

- Under <ss>Reference genome to display</ss> choose *Use a genome from history*.

- Under <ss>Select the reference genome</ss> choose <fn>wildtype.fna</fn>. This sequence will be the reference against which annotations are displayed.

- For <ss>Produce a Standalone Instance</ss> select *Yes*.

- For <ss>Genetic Code</ss> choose *11: The Bacterial, Archaeal and Plant Plastid Code*.

- Under <ss>JBrowse-in-Galaxy Action</ss> choose *New JBrowse Instance*.

- We will now set up three different tracks - these are datasets displayed underneath the reference sequence (which is displayed as nucleotides in FASTA format). We will choose to display the sequence reads (the .bam file), the variants found by snippy (the .gff file) and the annotated reference genome (the wildtype.gff)

*Track 1 - sequence reads*

- Click <ss>Insert Track Group</ss>
- For <ss>Track Cateogry</ss> name it "sequence reads"
- Click <ss>Insert Annotation Track</ss>
- For <ss>Track Type</ss> choose *BAM Pileups*
- For <ss>BAM Track Data</ss> select <fn>the snippy bam file</fn>
- For <ss>Autogenerate SNP Track</ss> select *Yes*
- Under <ss>Track Visibility</ss> choose *On for new users*.

*Track 2 - variants*

- Click <ss>Insert Track Group</ss> again
- For <ss>Track Category</ss> name it "variants"
- Click <ss>Insert Annotation Track</ss>
- For <ss>Track Type</ss> choose *GFF/GFF3/BED/GBK Features*
- For <ss>Track Data</ss> select <fn>the snippy snps gff file</fn>
- Under <ss>Track Visibility</ss> choose *On for new users*.

*Track 3 - annotated reference*

- Click <ss>Insert Track Group</ss> again
- For <ss> Track Category</ss> name it "annotated reference"
- Click <ss>Insert Annotation Track</ss>
- For <ss>Track Type</ss> choose *GFF/GFF3/BED/GBK Features*
- For <ss>Track Data</ss> select <fn>wildtype.gff</fn>
- Under <ss>JBrowse Track Type[Advanced]</ss> select *Canvas Features*.
- Click on <ss>JBrowse Styling Options <Advanced]</ss>
- Under <ss>JBrowse style.label</ss> add in the word *product*.
- Under <ss>JBrowse style.description</ss> add in the word *product*.
- Under <ss>Track Visibility</ss> choose *On for new users*.

- Click <ss>Execute</ss>

- A new file will be created, called <fn>JBrowse on data XX and data XX - Complete</fn>. Click on the eye icon next to the file name. The JBrowse window will appear in the centre Galaxy panel.

- On the left, tick boxes to display the tracks

- Use the minus button to zoom out to see:
    - sequence reads and their coverage (the grey graph)

- Use the plus button to zoom in to see:
    - probable real variants (a whole column of snps)
    - probable errors (single one here and there)

![JBrowse screenshot](images/jbrowse1.png)

- In the coordinates box, type in *47299* and then <ss>Go</ss> to see the position of the SNP discussed above.
    - the correct codon at this position is TGT, coding for the amino acid Cysteine, in the middle row of the amino acid translations.
    - the mutation of T &rarr; A turns this triplet into TGA, a stop codon.

![JBrowse screenshot](images/jbrowse2.png)    
