This README describes the alignment data available on the ftp site, how it is
processed and what summary information is available for each alignment.

###A. File format and alignment index

All alignment data is in the BAM format. This is the binary version of the SAM
format which is described here: [http://samtools.sourceforge.net/](http://samtools.sourceforge.net). Each BAM file
has two associated files: a index file which has the same name but ends with
.bai and a statistics file which has the same name but end with .bas. The format
of bas file is described further down in in section D.

The alignments are found under data/XXXXXXXX/alignment and data/XXXXXXX/exome_alignment
where the XXXXXXXX identifier is the sample name.

There is an alignment.index and an exome.alignment.index which links the alignment bam 
files together with their matching bai file and bas file and gives md5sums for each. 
The columns are as follows:
  1.	bam file
  2.	MD5
  3.	bai index file
  4.	bai index file md5
  5.	bas statistics file
  6.	bas statistic file md5

Directory alignment_indices/ contains the most current alignment.index (the one
with the latest date and identical to the alignment.index one level up), as well
as alignment.index files of previous BAM releases. The directory also contains
the following files about BAM statistics:

- yyyymmdd.alignment.index.bas.gz - a collective bas file of all BAM files.
- yyyymmdd_yyyymmdd.alignment_stats.csv - a summary statistics of BAMs in any two releases as specified by the dates in the file name; a comparison of the two releases is captured in the "diff" values of the file. The file contains the following information break down by platform. 
    1. mapped basepairs in Gb
    2. # of individuals in the release
    3. # of individuals with > 10 Gb of mapped sequences 

    Mapped basepairs in Gb is also shown as break down by population at the bottom of the file.
- 20091216.alignment_stats.csv - a summary statistics of the very first release of main project BAMs.

The exome alignments also have a HsMetrics files which contain the results from
the picard tool CalculateHsMetrics

The bam filenames themselves contain a lot of information, e.g:
NA12878.mapped.ILLUMINA.bwa.CEU.low_coverage.20121211.bam

The name can be broken down into 7 pieces:
1. Sample name: this matches column 10 in the sequence.index file and represents the individual all the sequences belong to.
2. Region, this is generally either mapped or unmapped, the mapped files represents all the mapping to the reference genome, the unmapped file represents all the unmapped reads. You may also see chromN files which represent mappings to just that chromosome.
3. Sequencing platform: all our current release should be ILLUMINA but old alignment files will have SOLID or LS454.
4. Mapping algorithm: For ILLUMINA this is bwa, again older files may have bfast for solid	or ssaha or smalt for 454. 
5. Population*: the Sample population 3 letter code, this code is defined in README.populations.
6. Analysis group*: this describes the sequencing strategy used for the sequence being aligned, those strategies being 'low coverage', 'high coverage', 'exon targeted' and 'exome'.
7. Date in the format YYYYMMDD: this should match the date from the sequence.index file which was used to produce the alignments. This date will also be in the alignment index name. Over time one release of alignments will contain multiple index dates. The index itself is named for the most recent sequence.index it is based on. The index in the bam file name reflects the last sequence index new data was added for that individual.

**Note:** For historical reasons, in the first release of BAM files, fields 5 and 6
are replaced with a single column of project names such as SRP000033. One example
is NA20828.chrom7.ILLUMINA.bwa.SRP000033.20091216.bam

###B. Alignment Process

All the most recent alignments were produced by Richard Durbin's group at the Sanger
based on our analysis.sequence.index file which contains all our ILLUMINA sequence data
which has 70bp or longer reads. Older alignment releases also contain SOLID and 454 sequence
data and their alignment process is explained in the README which sits with those
alignments

**Illumina data was aligned with bwa v0.5.9 in 4 steps:**

1.  Index the reference fasta:

         bwa index -a bwtsw $reference_fasta

2.  For each fastq file:

         bwa aln -q 15 -f $sai_file $reference_fasta $fastq_file

3.  Create SAM files using bwa sampe or samse for paired-end or unpaired reads respectively. For paired-end reads, the maximum insert size is taken to be 3 times the expected insert size.

         bwa sampe -a $max_insert_size -f $sam_file $reference_fasta $sai_files $fastq_files
         bwa samse -f $sam_file $reference_fasta $sai_file $fastq_file

4.  Create BAM from SAM, sort, fix mate information and add the MD tag:

         samtools view -bSu $sam_file | samtools sort -n -o - samtools_nsort_tmp | 
         samtools fixmate /dev/stdin /dev/stdout | samtools sort -o - samtools_csort_tmp |
         samtools fillmd -u - $reference_fasta > $fixed_bam_file
  

**Bam Improvement**
  
The run-level alignment BAMs are improved in various ways to help increase the quality and speed of subsequent SNP calling that may be carried out on them. For Illumina BAMs the following improvements were performed:

1.  Reads undergo local realignment around known Indels using GATK IndelRealigner.

         java $jvm_args -jar GenomeAnalysisTK.jar -T RealignerTargetCreator -R $reference_fasta -o $intervals_file -known $known_indels_file(s)
         java $jvm_args -jar GenomeAnalysisTK.jar -T IndelRealigner -R $reference_fasta -I $bam_file -o $realigned_bam_file -targetIntervals $intervals_file -known $known_indels_file(s) -LOD 0.4 -model KNOWNS_ONLY -compress 0 --disable_bam_indexing

    where the known Indels are from the following:

         ftp://ftp.1000genomes.ebi.ac.uk/vol1/ftp/technical/reference/phase2_mapping_resources/ALL.wgs.indels_mills_devine_hg19_leftAligned_collapsed_double_hit.indels.sites.vcf.gz
         ftp://ftp.1000genomes.ebi.ac.uk/vol1/ftp/technical/reference/phase2_mapping_resources/ALL.wgs.low_coverage_vqsr.20101123.indels.sites.vcf.gz

2.  Realigned BAMs have their read qualities recalibrated with GATK CountCovariates and TableRecalibration.

         java $jvm_args -jar GenomeAnalysisTK.jar -T CountCovariates -R $reference_fasta -I $realigned_bam_file -recalFile recal_data.csv -knownSites $known_sites_file(s) -l INFO -L '1;2;3;4;5;6;7;8;9;10;11;12;13;14;15;16;17;18;19;20;21;22;X;Y;MT' -cov ReadGroupCovariate -cov QualityScoreCovariate -cov CycleCovariate -cov DinucCovariate
         java $jvm_args -jar GenomeAnalysisTK.jar -T TableRecalibration -R $reference_fasta -recalFile recal_data.csv -I $realigned_bam_file -o $recalibrated_bam_file -l INFO -compress 0 --disable_bam_indexing

    where the known sites for recalibration are from dbSNP135:
   [ftp://ftp.1000genomes.ebi.ac.uk/vol1/ftp/technical/reference/phase2_mapping_resources/ALL.wgs.dbsnp.build135.snps.sites.vcf.gz](ftp://ftp.1000genomes.ebi.ac.uk/vol1/ftp/technical/reference/phase2_mapping_resources/ALL.wgs.dbsnp.build135.snps.sites.vcf.gz)

3.  samtools calmd is applied to the recalibrated BAMs, which fixes the NM tags and introduces BQ tags which can be used during SNP calling.

         samtools calmd -Erb $recalibrated_bam_file $reference_fasta > $bq_bam_file

    For Exome Solid BAMs, only step 2 was performed.
    For low coverage Solid BAMs, only steps 1 and 2 were performed.

**Release bam file production**

The improved BAMs are merged together to get the release BAM files available 
for download. Release BAM files therefore contain reads from multiple 
readgroups. Production proceeds broadly as follows:

1.  Run-level BAMs have extraneous tags (OQ, XM, XG, XO) stripped from them, to reduce total file size by around 30%. 

2.  Tag-stripped BAMs are merged to the library level with Picard. (merging to a given level means to create a single BAM file containing all the readgroups that share a given member of that level; in this case it means a BAM will be made for each library, each library BAM containing all the reads from the run-level BAMs associated with that library).

         java $jvm_args -jar MergeSamFiles.jar INPUT=$tag_stripped_bam_file(s) OUTPUT=$library_level_bam VALIDATION_STRINGENCY=SILENT

3.  PCR duplicates are marked in library-level BAMs using Picard MarkDuplicates.

         java $jvm_args -jar MarkDuplicates.jar INPUT=$library_level_bam OUTPUT=$markdup_bam_file ASSUME_SORTED=TRUE METRICS_FILE=/dev/null VALIDATION_STRINGENCY=SILENT

4.  Duplicate-marked library-level BAMs are merged to the platform level.

         java $jvm_args -jar MergeSamFiles.jar INPUT=$markdup_bam_file(s) OUTPUT=$platform_level_bam VALIDATION_STRINGENCY=SILENT

5.  Platform-level BAMs are split into chromosomes (and other, see above) BAMs, which are the release files.

###C.  QA of the alignment data by DCC

Platform-level bams for individuals are checked by a QA process by DCC before they are released to the ftp site. Here lists the QA measurements and criteria:

1. Check md5 for each file to make sure the file was not corrupted during transfer.
 
2. Check coverage status. For exome runs this is done by runnung calculate_HsMetrics (a function in PICARD) to evaluate EXOME sequence coverage. Samples with less than 70% 20x coverage in targetted regions are considered coverage too low. The bait file used in this calculation is [ftp://ftp.1000genomes.ebi.ac.uk/vol1/ftp/technical/reference/exome_pull_down_targets_phases1_and_2/20120518.consensus.annotation.bed](ftp://ftp.1000genomes.ebi.ac.uk/vol1/ftp/technical/reference/exome_pull_down_targets_phases1_and_2/20120518.consensus.annotation.bed)

For low coverage samples we use the bas files supplied with the bams. These contain
the number of mapped bases and the number of duplicate bases. We calculate the non
duplicated mapped coverage. This must be 3x or greater (consdering a genome size of 2.75Gb)

3. Check for excessive number of short insertions or deletions (<=6bp). If the ratio of 
short insertion counts to short deletion counts is greater than 5, the sample fails. 
The reverse ratio of short deletion counts to short insertion counts is also checked 
the same way. (This uses a custom script, if you would like a copy please email info@1000genomes.org)
 
4. Check for sample contamination using VerifyBAMId developed by Hyun Kang in University of Michigan.  
VerifyBamId is a software that verifies whether the reads in particular file match previously 
known genotypes for an individual (or group of individuals), and checks whether the reads are 
contaminated as a mixture of two samples. verifyBamID can detect sample contamination and swaps 
when external genotypes are available. When external genotypes are not available, verifyBamID still 
robustly detects sample swaps. Genotype data for most phase1 and 2 samples typed on OMNI chips and 
genotype data for most phase3 samples typed on Affymetrix chips were used in this QA process. The original OMNI
genotype vcf files can be found at [ftp://ftp.1000genomes.ebi.ac.uk/vol1/ftp/technical/working/20120131_omni_genotypes_and_intensities/Omni25_genotypes_2141_samples.b37.vcf.gz](ftp://ftp.1000genomes.ebi.ac.uk/vol1/ftp/technical/working/20120131_omni_genotypes_and_intensities/Omni25_genotypes_2141_samples.b37.vcf.gz)
and Affy genotype data can be found at [ftp://ftp.1000genomes.ebi.ac.uk/vol1/ftp/technical/working/20121128_corriel_p3_sample_genotypes/](ftp://ftp.1000genomes.ebi.ac.uk/vol1/ftp/technical/working/20121128_corriel_p3_sample_genotypes/). We divided the vcf files by sample
population when ran VerifyBamId.
The CHIP_MIX and FREE_MIX cutoff ranges from 2-3.5% for different BAM releases.
[http://genome.sph.umich.edu/wiki/VerifyBamID](http://genome.sph.umich.edu/wiki/VerifyBamID)
 
5. Check for completeness of data against sequence index:
   a.  All reads and runs for an individual should be included incorresponding BAM files
   b.  All reads and runs from a BAM file should be from the same individual

It is important to note that the 20130502 bam release was unusual as rather than all QC passed bams
did not make it into the data/XXXXXX/alignment directories

For the final round of analysis for the project we only used ILLUMINA sequence data which was 70bp or greater.
The group decided for the final variant calling only samples with both low coverage and exome sequence would
be considered. This means there were a small number of samples with just exome or just low coverage alignments.
The sequence data is still included in the sequence.index file and while the alignments are not being used in
our variant calling they are still available to be downloaded. 

[ftp://ftp.1000genomes.ebi.ac.uk/vol1/ftp/technical/phase3_EX_or_LC_only_alignment/](ftp://ftp.1000genomes.ebi.ac.uk/vol1/ftp/technical/phase3_EX_or_LC_only_alignment/)

These bam files have their own alignment index which you can find in this directory.

###D. bas files

.bas files are bam statistic files, one line per readgroup, columns separated by
tabs. The first line is a header that describes each column. The first six columns
provide meta information about each readgroup.

The remaining columns provide various statistics about the readgroup, calculated
by going through the release bams. Where data isn't available to calculate the
result for a column the default value will be 0.

Each column is described in detail below.

   Column 1 'bam_filename': The DCC bam file name in which the readgroup data can be found.

   Column 2 'md5': The md5 checksum of the bam file named in column 1.

   Column 3 'study': The SRA study id this readgroup belongs to.

   Column 4 'sample': The sample (individual) identifier the readgroup came from.

   Column 5 'platform': The sequencing platform (technology) used to sequence the readgroup.

   Column 6 'library': The name of the library used for the readgroup.

   Column 7 'readgroup': The readgroup identifier. This is unique per .bas file. The remaining columns summarise data for reads with this RG tag in the bam file given in column 1.

   Column 8 '#_total_bases': The sum of the length of all reads in this readgroup.

   Column 9 '#_mapped_bases': The sum of the length of all reads in this readgroup that did not have flag 4 (== unmapped).

   Column 10 '#_total_reads': The total number of reads in this readgroup.

   Column 11 '#_mapped_reads': The total number of reads in this readgroup that did
    not have flag 4 (== unmapped).

   Column 12 '#_mapped_reads_paired_in_sequencing': As for column 10, but also requiring flag 1 (== reads paired in sequecing).

   Column 13 '#_mapped_reads_properly_paired': As for column 10, but also requiring flag 2 (== mapped in a proper pair, inferred during alignment).

   Column 14 '%_of_mismatched_bases': Calculated by summing the read lengths of all
    reads in this readgroup that have an NM tag, summing the edit distances
    obtained from the NM tags, and getting the percentage of the latter out of
    the former to 2 decimal places.

   Column 15 'average_quality_of_mapped_bases': The mean of all the base qualities
    of the bases counted for column 8, to 2 decimal places.

   Column 16 'mean_insert_size': The mean of all insert sizes (ISIZE field) greater
    than 0 for properly paired reads (as counted in column 12) and with a
    mapping quality (MAPQ field) greater than 0. Rounded to the nearest whole
    number.

   Column 17 'insert_size_sd': The standard deviation from the mean of insert sizes
    considered for column 15. To 2 decimal places.

   Column 18 'median_insert_size': The median insert size, using the same set of
    insert sizes considered for column 15.

   Column 19 'insert_size_median_absolute_deviation': The median absolute deviation
    of the column 17 data.

   Column 20 '#_duplicate_reads': The number of reads which were marked as
    duplicates

   Column 21' #_duplicate_bases': The number of bases which were narked as
    duplicated

