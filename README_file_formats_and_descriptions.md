#File formats and descriptions

This file provides information on some of the file formats used to make data available on this site.

###CRAM
.cram files use a reference-based format to store data. This format is being used to supply alignment data on this site. Detailed information on working with our .cram files is provided in ftp://ftp.1000genomes.ebi.ac.uk/vol1/ftp/README_using_1000genomes_cram.md.

###CRAI
.crai files are index files which accompany CRAM files. They must be present in order to work with .cram files for most purposes.

###BAS
.bas files are .cram or .bam statistic files with one line per readgroup and columns separated by
tabs. The first line is a header that describes each column. The first six columns
provide meta information about each readgroup.

The remaining columns provide various statistics about the readgroup, calculated
by going through the release bams. Where data isn't available to calculate the
result for a column, the default value will be 0.

Each column is described in detail below:

             Column 1  'bam_filename': The DCC bam file name in which the readgroup data can be found.
             Column 2  'md5': The md5 checksum of the bam file named in column 1.
             Column 3  'study': The SRA study id this readgroup belongs to.
             Column 4  'sample': The sample (individual) identifier the readgroup came from.
             Column 5  'platform': The sequencing platform (technology) used to sequence the readgroup.
             Column 6  'library': The name of the library used for the readgroup.
             Column 7  'readgroup': The readgroup identifier. This is unique per .bas file. The remaining columns summarise data for reads with this RG tag in the bam file given in column 1.
             Column 8  '#_total_bases': The sum of the length of all reads in this readgroup.
             Column 9  '#_mapped_bases': The sum of the length of all reads in this readgroup that did not have flag 4 (== unmapped).
             Column 10 '#_total_reads': The total number of reads in this readgroup.
             Column 11 '#_mapped_reads': The total number of reads in this readgroup that did not have flag 4 (== unmapped).
             Column 12 '#_mapped_reads_paired_in_sequencing': As for column 10, but also requiring flag 1 (== reads paired in sequecing).
             Column 13 '#_mapped_reads_properly_paired': As for column 10, but also requiring flag 2 (== mapped in a proper pair, inferred during alignment).
             Column 14 '%_of_mismatched_bases': Calculated by summing the read lengths of all reads in this readgroup that have an NM tag, summing the edit distances obtained from the NM tags, and getting the percentage of the latter out of the former to 2 decimal places.
             Column 15 'average_quality_of_mapped_bases': The mean of all the base qualities of the bases counted for column 8, to 2 decimal places.
             Column 16 'mean_insert_size': The mean of all insert sizes (ISIZE field) greater than 0 for properly paired reads (as counted in column 12) and with a mapping quality (MAPQ field) greater than 0. Rounded to the nearest whole number.
             Column 17 'insert_size_sd': The standard deviation from the mean of insert sizes considered for column 15. To 2 decimal places.
             Column 18 'median_insert_size': The median insert size, using the same set of insert sizes considered for column 15.
             Column 19 'insert_size_median_absolute_deviation': The median absolute deviation of the column 17 data.
             Column 20 '#_duplicate_reads': The number of reads which were marked as duplicates.
             Column 21 '#_duplicate_bases': The number of bases which were narked as duplicated

###INDEX
Various types of index file exist on the site. These are tab-delimited files where the data is arranged in columns. Immediately before the body of the file there is a header line, which starts with #, that gives the column names. In addition, index files may have further information at the head of the file. These lines start with ## and can provide descriptions of the columns, the date the index was generated and other pieces of information, as appropriate to the file and data set.

An example of the start of such a file, in this case an alignment index file, is below:

         ##FileDate=20150914
         ##Project=Illumina Platinum pedigree
         ##CRAM_FILE=Path to CRAM file - information on CRAM files can be found in ftp://ftp.1000genomes.ebi.ac.uk/vol1/ftp/README_using_1000genomes_cram.md and information on the alignment process accompanies the data collection
         ##CRAM_MD5=md5 for CRAM file
         ##CRAI_FILE=Path to CRAI file
         ##CRAI_MD5=md5 for CRAI file
         ##BAS_FILE=Path to BAS file
         ##BAS_MD5=md5 for BAS file
         #CRAM_FILE	CRAM_MD5	CRAI_FILE	CRAI_MD5	BAS_FILE	BAS_MD5
         ftp://ftp.1000genomes.ebi.ac.uk/vol1/ftp/data_collections/illumina_platinum_pedigree/data/CEU/NA12893/alignment/NA12893.alt_bwamem_GRCh38DH.20150706.CEU.illumina_platinum_ped.cram	4aa7f5b61d4365a556c980278b9be5a1	ftp://ftp.1000genomes.ebi.ac.uk/vol1/ftp/data_collections/illumina_platinum_pedigree/data/CEU/NA12893/alignment/NA12893.alt_bwamem_GRCh38DH.20150706.CEU.illumina_platinum_ped.cram.crai	ae3d1ac0de67d58d192a96508bad85b4	ftp://ftp.1000genomes.ebi.ac.uk/vol1/ftp/data_collections/illumina_platinum_pedigree/data/CEU/NA12893/alignment/NA12893.alt_bwamem_GRCh38DH.20150706.CEU.illumina_platinum_ped.bam.bas	7fb61b29c6b5fc716e9167affab56d92
         ftp://ftp.1000genomes.ebi.ac.uk/vol1/ftp/data_collections/illumina_platinum_pedigree/data/CEU/NA12892/alignment/NA12892.alt_bwamem_GRCh38DH.20150706.CEU.illumina_platinum_ped.cram	8acdcd17349546b5ca1b45111e30fc07	ftp://ftp.1000genomes.ebi.ac.uk/vol1/ftp/data_collections/illumina_platinum_pedigree/data/CEU/NA12892/alignment/NA12892.alt_bwamem_GRCh38DH.20150706.CEU.illumina_platinum_ped.cram.crai	555de96d6baf2e16630dadd9a6b5b038	ftp://ftp.1000genomes.ebi.ac.uk/vol1/ftp/data_collections/illumina_platinum_pedigree/data/CEU/NA12892/alignment/NA12892.alt_bwamem_GRCh38DH.20150706.CEU.illumina_platinum_ped.bam.bas	5079d6d9dfcb0eb4631d11035ec71b16

###Further information
For further information, please contact info@1000genomes.org.
