# Nanopore tutorial

We will be using the Galaxy server that you have been using this week so far so please open that up.

## Data we will use ([_download_]())

* <FN>EM_079517.fasta</FN>
* <FN>illumina_assembly_graph.fastg</FN>
* <FN>O55_combined_miniasm.gfa</FN>
* <FN>sakai_prophages.fa</FN>

## Non-Galaxy tools needed ([_download_](https://www.dropbox.com/sh/kvvkvmhbwzovcg2/AAASrlyOWSq2jt-IQKyPjBgZa/Software_to_install?dl=0))

* [Tablet](https://ics.hutton.ac.uk/tablet/) 
* [Bandage](http://rrwick.github.io/Bandage/)

## Introduction

First off, we are going to start by using some Ebola viral genome data to look at
mapping (aligning) Nanopore reads to a reference.  
We are going to use `bwa mem` that has long read capability.

## Step 1: Fastq-dump
First we need to retrieve Ebola data from NCBI. 

* We will use the <SS>Get Data</SS> tab on the left hand side panel of Galaxy, under <SS>Tools</SS>. 
* Click on <SS>Download and Extract Reads in FASTA/Q</SS>
* Options will now be visible in the middle panel of galaxy
* The input type should be <SS>SRR accession</SS>
* In Accession enter <SS>ERR1014225</SS> (This is the accession number for nanopore reads of an Ebola sample from the West African outbreak)
* Select <SS>gzip compressed fastq</SS> for the output format
* Click <SS>Execute</SS>

![Alt text](screenshots/Screen%20Shot%202018-06-07%20at%209.52.33%20AM.png)

* The job will then appear in the <SS>History</SS> panel on the right hand side of the screen

![Alt text](screenshots/Screen%20Shot%202018-06-07%20at%2010.07.13%20AM.png)

* The job will turn green when it has finished

## Step 2: BWA-MEM
Next we will map our nanopore Ebola reads to a reference.

* We will use the <SS>NGS:Mapping</SS> tab on the left hand side panel of Galaxy, under <SS>Tools</SS>. 
* Click on <SS>Map with BWA-MEM</SS>
* Options will now be visible in the middle panel of galaxy
* Select <SS>Use a genome from history and build index</SS>
* The reference sequence will be preloaded into your galaxy history: <FN>EM_079517.fasta</FN> (This is a finished Ebola reference genome for strain Zaire)
* For Algorithm select <SS>Auto. Let BWA decide the best algorithm to use</SS>
* Select <SS>Single</SS> for type of reads
* Select <SS>ERR1014225 (fastq-dump)</SS> for fastq dataset
* Select <SS>Do not set</SS> for read groups information
* Select <SS>3. Nanopore 2D-reads mode (-x ont2d)</SS> for analysis mode  
* Click <SS>Execute</SS>

![Screenshot](screenshots/Screen%20Shot%202018-06-07%20at%2010.07.13%20AM.png)

When it has finished running, we will download the mapped reads.

* Click on the finished job (Called <SS>Map with BWA-MEM</SS>etc.) in the <SS>History</SS> panel on the right hand side panel 
* You will see a save icon under the details of the job
* Click on it and the <SS>Download dataset</SS> and <SS>Download bam_index</SS> options will appear
* We need both so click on one after the other 

![Alt text](screenshots/Screen%20Shot%202018-06-07%20at%2010.32.52%20AM.png)

## Step 3: Viewing the mapping in Tablet

* We will load the downloaded data into Tablet by opening the Tablet program and clicking on <SS>Open Assembly</SS> in the top left hand side of the screen
* For <SS>Primary assembly file or URL:</SS> click on <SS>Browse...</SS> and find the downloaded Bam file (you need the file with the extension <FN>.bam</FN> not <FN>.bai</FN>)
* For <SS>Reference/consensus file or URL:</SS> click on <SS>Browse...</SS> and find the downloaded (from dropbox) reference genome <FN>EM_079517.fasta</FN>

![Alt text](screenshots/Screen%20Shot%202018-06-07%20at%2010.42.02%20AM.png)

* Click <SS>Open</SS> and then click on the Contig <FN>EM_079517</FN> on the left hand panel of the screen and you will see your mapping results ready to browse
* Take a few minutes to explore the program and the results

![Alt text](screenshots/Screen%20Shot%202018-06-07%20at%2010.44.03%20AM.png)

What can you tell from the alignment about the method of sequencing?

Does the data look different to Illumina data that you might have seen?

Can you find anything that looks like consistent errors?

Can you find anything that looks like a reliable SNP? 

## Step 4: Comparing Illumina and Nanopore assemblies

Next, we're going to use *Escherichia coli* to illustrate how much assemblies can be improved with long reads

## Pre-knowledge

* Pathogenic *E. coli* tends to have a lot repeat sequences
* This is due to large prophages integrating in the genome
* These prophages are very similar to each other and cause confusion for assembly algorithms 
* These prophages can be as large as ~50kb
* No matter how deep you sequence, if you don't get reads longer than those repeats you will never finish the genome
* See [this paper](https://genomebiology.biomedcentral.com/articles/10.1186/gb-2013-14-9-r101) for more detail

## Bandage

	
	Nodes = contigs
	Edges = overlaps

* Open up Bandage from your desktop
* Click on <SS>File</SS> then <SS>Load graph</SS>
* Find the <SS>illumina_assembly_graph.fastg</SS> location on your computer and click <SS>Open</SS>

![Alt text](screenshots/Screen%20Shot%202018-06-07%20at%2011.20.11%20AM.png)
 
 * Click <SS>Draw graph</SS> on the left hand panel of the screen
 * You will see that the Illumina assembly produces a whole mess of 649 contigs with a lot of short contigs connecting everything together
 * You can zoom in on the graph to look at it in more detail
 
![Alt text](screenshots/Screen%20Shot%202018-06-07%20at%2012.40.28%20PM.png)

 * We can colour the graph by depth by changing <SS>Graph display</SS> from <SS>Random colours</SS> to <SS>Colour by depth</SS>
 * You'll see that the short contigs all have much higher coverage than the other contigs. *What do you think that means?*

![Alt text](screenshots/Screen%20Shot%202018-06-07%20at%2012.47.48%20PM.png)

Now we are going to use [BLAST](https://blast.ncbi.nlm.nih.gov/Blast.cgi) to look for the prophage regions in the assembly

* Click on <SS>Create/view BLAST search</SS> in the left hand panel of the screen
* <SS>Step 1</SS> click on <SS>Build BLAST database</SS>
* <SS>Step 2</SS> click on <SS>Load from FASTA file</SS> and find the <FN>sakai_prophages.fa</FN> location on your computer
* <SS>Step 3</SS> click on <SS>Run BLAST search</SS>
* click on <SS>Close</SS>

![Alt text](screenshots/Screen%20Shot%202018-06-07%20at%2012.52.56%20PM.png)

You will see the prophage regions highlighted in colour on the assembly graph now.

What affect are the prophage regions having on the assembly?

![Alt text](screenshots/Screen%20Shot%202018-06-07%20at%2012.54.03%20PM.png)

Now, we will load the nanopore assembly into Bandage to see how long reads affect this

* Click on <SS>File</SS> then <SS>Load graph</SS>
* Find the <FN>O55_combined_miniasm.gfa</FN> location on your computer and click <SS>Open</SS>
* Click <SS>Draw graph</SS> on the left hand panel of the screen
* Change <SS>Graph display</SS> option from <SS>BLAST hits(solid)</SS> to <SS>Random colours</SS>. *What is strikingly different about this assembly than the Illumina one?*

![Alt text](screenshots/Screen%20Shot%202018-06-07%20at%2012.58.17%20PM.png)

Now, lets look at where the prophage regions are in this assembly

* Click on <SS>Create/view BLAST search</SS> in the left hand panel of the screen
* <SS>Step 1</SS> click on <SS>Build BLAST database</SS>
* <SS>Step 2</SS> click on <SS>Load from FASTA file</SS> and find the <FN>sakai_prophages.fa</FN> location on your computer
* <SS>Step 3</SS> click on <SS>Run BLAST search</SS>
* click on <SS>Close</SS>

![Alt text](screenshots/Screen%20Shot%202018-06-07%20at%2012.59.42%20PM.png)

1. How have the long reads improved this assembly?
2. Are there any regions that haven't been resolved by the nanopore experiment? Why would that be?