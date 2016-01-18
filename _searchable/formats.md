---
layout: single_section
title: "formats"
permalink: /formats/
---
# Data file formats

## Alignment files: BAM and CRAM

BAM files are binary representations of the Sequence Alignment/Map format. These files and the associated SAMtools package are described in this [Bioinformatics publication](http://bioinformatics.oxfordjournals.org/cgi/content/abstract/25/16/2078).

Additional information about SAM/BAM is available at the SAMtools development site: [http://samtools.sourceforge.net/](http://samtools.sourceforge.net/).

CRAM files are similar to BAM files but give a compressed representation of the alignment. This compression is driven by the reference the sequence data is aligned to. The file format was designed to reduce the disk foot print of alignment data by the [EBI, who provide further information on the format](http://www.ebi.ac.uk/ena/software/cram-toolkit).

## Variant Call Format (VCF)

The VCF format is a tab delimited format for storing variant calls and individual genotypes. It is able to store all variant calls from single nucleotide variants to large scale insertions and deletions.

##Data file format specifications

The specifications for these file formats continue to develop. Current specifications for [SAM/BAM, CRAM and VCF can be found at hts-specs](https://samtools.github.io/hts-specs/).

# Key file formats

Information on key file formants is normally provided within README files located near the relevant data on the FTP site. Information on the major formats is also collected here. Tools developed by the 1000 Genomes project or appropirate to work with the data from t he project are listed on the [tools](/tools) page.

# Index file formats

## Sequence index

The sequence.index file is a tab delimited file containing all the meta data you should need to download and subset the files on this ftp site by individual, library, experiment and sequencing technology.

The columns are:

1\. FASTQ_FILE, path to fastq file on ftp site  
2\. MD5, md5sum of file  
3\. RUN_ID, SRA/ERA run accession  
4\. STUDY_ID, SRA/ERA study accession  
5\. STUDY_NAME, Name of stury  
6\. CENTER_NAME, Submission centre name  
7\. SUBMISSION_ID, SRA/ERA submission accession  
8\. SUBMISSION_DATE, Date sequence submitted, YYYY-MM-DAY  
9\. SAMPLE_ID, SRA/ERA sample accession  
10\. SAMPLE_NAME, Sample name  
11\. POPULATION, Sample population  
12\. EXPERIMENT_ID, Experiment accession  
13\. INSTRUMENT_PLATFORM, Type of sequencing machine  
14\. INSTRUMENT_MODEL, Model of sequencing machine  
15\. LIBRARY_NAME, Library name  
16\. RUN_NAME, Name of machine run  
17\. RUN_BLOCK_NAME, Name of machine run sector  
18\. INSERT_SIZE, Submitter specifed insert size  
19\. LIBRARY_LAYOUT, Library layout, this can be either PAIRED or SINGLE  
20\. PAIRED_FASTQ, Name of mate pair file if exists (Runs with failed mates will have a library layout of PAIRED but no paired fastq file)  
21\. WITHDRAWN, 0/1 to indicate if the file has been withdrawn, only present if a file has been withdrawn  
22\. WITHDRAWN_DATE, date of withdrawal, this should only be defined if a file is withdrawn  
23\. COMMENT, comment about reason for withdrawal  
24\. READ_COUNT, read count for the file  
25\. BASE_COUNT, basepair count for the file  
26\. ANALYSIS_GROUP, the analysis group of the sequence, this reflects sequencinq strategy. Current this includes low coverage, high coverage and exon targetted to reflect the 3 stratergies used by the 1000 genomes project.

Any run_id can have up to 3 files associated with it. Single runs have one file. Paired runs can have anywhere from 1 to 3 files depending on the success of the pairing.

## Alignment Index

The alignment.index file is a tab delimited file containing all the meta data needed to download the bam alignment files from the ftp site. The alignment file formants are described [below](#DataFiles).

The columns are:

1\. bam file  
2. MD5  
3. bai index file  
4. bai index file md5  
5\. bas statistics file  
6\. bas statistic file md5

All other meta information associated with a particular file can be found in the filename e.gHG00098.chrom1.ILLUMINA.bwa.GBR.low_coverage.20100804.bam

The fields represent:

Sample Name  
Chromosome name, nonchrom represents the reads mapped to non chromosomal parts of the reference genome and unmapped represents readpairs which did not map to the reference genome  
Instrument Platform, the sequencing platform the sequence comes from.  
Algorithm, the algorithm used to map the data  
Population, the population the sample comes from  
Analysis group, the sequencing strategy used to generate the sequence data  
Sequence index date, the date from the sequence.index used to supply the input sequence data for the alignment

Each bam file is paired with an index file which has the same name but the extension .bai and a statistic file which has the extension .bas. More information about the files can be found in [README.alignment_data](ftp://ftp.1000genomes.ebi.ac.uk/vol1/ftp/README.alignment_data)

The sequence files all use the reference genome which can be found under:  
[ftp://ftp.1000genomes.ebi.ac.uk/vol1/ftp/technical/reference/](ftp://ftp.1000genomes.ebi.ac.uk/vol1/ftp/technical/reference/)  
[ftp://ftp-trace.ncbi.nih.gov/1000genomes/ftp/technical/reference/](ftp://ftp-trace.ncbi.nih.gov/1000genomes/ftp/technical/reference/)

## Bas File

.bas files are bam statistic files, one line per readgroup, columns separated by tabs. The first line is a header that describes each column. The first 6 columns provide meta information about each readgroup.

The remaining columns provide various statistics about the readgroup, calculated by going through the release bams. Where data isn't available to calculate the result for a column the default value will be 0.

Each column is described in detail below.

Column 1 'bam_filename': The DCC bam file name in which the readgroup data can be found.

Column 2 'md5': The md5 checksum of the bam file named in column 1.

Column 3 'study': The SRA study id this readgroup belongs to.

Column 4 'sample': The sample (individual) identifier the readgroup came from.

Column 5 'platform': The sequencing platform (technology) used to sequence the readgroup.

Column 6 'library': The name of the library used for the readgroup.

Column 7 'readgroup': The readgroup identifier. This is unique per .bas file.

The remaining columns summarise data for reads with this RG tag in the bam file given in column 1.

Column 8   '#_total_bases': The sum of the length of all reads in this readgroup.

Column 9   '#_mapped_bases': The sum of the length of all reads in this readgroup that did not have flag 4 (== unmapped).

Column 10 '#_total_reads': The total number of reads in this readgroup.

Column 11 '#_mapped_reads': The total number of reads in this readgroup that did not have flag 4 (== unmapped).

Column 12 '#_mapped_reads_paired_in_sequencing': As for column 10, but also requiring flag 1 (== reads paired in sequencing).

Column 13 '#_mapped_reads_properly_paired': As for column 10, but also requiring flag 2 (== mapped in a proper pair, inferred during alignment).

Column 14 '%_of_mismatched_bases': Calculated by summing the read lengths of all reads in this readgroup that have an NM tag, summing the edit distances obtained from the NM tags, and getting the percentage of the latter out of the former to 2 decimal places.


Column 15 'average_quality_of_mapped_bases': The mean of all the base qualities of the bases counted for column 8, to 2 decimal places.

Column 16 'mean_insert_size': The mean of all insert sizes (ISIZE field) greater than 0 for properly paired reads (as counted in column 12) and with a mapping quality (MAPQ field) greater than 0\. Rounded to the nearest whole number.

Column 17 'insert_size_sd': The standard deviation from the mean of insert sizes considered for column 15\. To 2 decimal places.

Column 18 'median_insert_size': The median insert size, using the same set of insert sizes considered for column 15.

Column 19 'insert_size_median_absolute_deviation': The median absolute deviation of the column 17 data.

Column 20 '#_duplicate_reads': The number of reads which were marked as duplicates


