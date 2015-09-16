This readme describes the sequence data on the ftp site, how it is processed
and what information is available about the sequence data.

Directory structure and sequence index file:
	
All sequence data is in the format fastq. This gives a sequence and a quality 
string for each read.

The sequences are found under /data/XXXXXX/sequence_read where XXXXXX represents the
sample name, this should be in the form or coriell sample names HGXXXXX or NAXXXXX

The meta data associated with a particular file including its md5sum can be found
in the sequence.index file. This is a tab delimted file where each column contains
a different piece of meta information.

We also present an analysis.sequence.index file which contains all the same information as the
sequence.index file but only refer to ILLUMINA data with 70bp reads or longer. For phase 3 of the 
project this is what we will analyse.

The columns of the sequence index file are

	1.  FASTQ_FILE, path to fastq file on ftp site  
	2.  MD5, md5sum of file
	3.  RUN_ID, SRA/ERA run accession       
	4.  STUDY_ID, SRA/ERA study accession   
	5.  STUDY_NAME, Name of stury   
	6.  CENTER_NAME, Submission centre name 
	7.  SUBMISSION_ID, SRA/ERA submission accession 
	8.  SUBMISSION_DATE, Date sequence submitted, YYYY-MM-DAY       
	9.  SAMPLE_ID, SRA/ERA sample accession 
	10. SAMPLE_NAME, Sample name    
	11. POPULATION, Sample population, this is a 3 letter code and it is defined in README.populations     
	12. EXPERIMENT_ID, Experiment accession 
	13. INSTRUMENT_PLATFORM, Type of sequencing machine     
	14. INSTRUMENT_MODEL, Model of sequencing machine       
	15. LIBRARY_NAME, Library name  
	16. RUN_NAME, Name of machine run       
	17. RUN_BLOCK_NAME, Name of machine run sector  (This is no longer recorded so this column is entirely null, it was left in so
            as not to disrupt existing sequence index parsers)
	18. INSERT_SIZE, Submitter specifed insert size 
	19. LIBRARY_LAYOUT, Library layout, this can be either PAIRED or SINGLE 
	20. PAIRED_FASTQ, Name of mate pair file if exists (Runs with failed mates will have 
	    a library layout of PAIRED but no paired fastq file)
	21. WITHDRAWN, 0/1 to indicate if the file has been withdrawn, only present if a file has been withdrawn
	22. WITHDRAWN_DATE This is generally the date the file is generated on
	23. COMMENT, comment about reason for withdrawal
	24. READ_COUNT, read count for the file
	25. BASE_COUNT, basepair count for the file
	26. ANALYSIS_GROUP, the analysis group of the sequence, this reflects sequencing
	    strategy. Currently this includes low coverage, high coverage, exon targetted and exome
	    to reflect the 2 non low coverage pilot sequencing stratergies and the 2 main project sequencing stratergies used by the 
	    1000 genomes project. 

Any run_id can have up to 3 files associated with it. Single runs have one file. 
Paired runs can have anywhere from 1 to 3 files depending on the success of the 
pairing. The paired run files are associated as *_1.fastq and *_2.fastq and any reads where one
of the two reads failed the qc will be found in the unnumbered fragment file

There is a record of all sequence index files created in 

ftp://ftp.1000genomes.ebi.ac.uk/vol1/ftp/sequence_indices/

Each file is dated for its release using the form YYYYMMDD.sequence.index 
so 20091216.sequence.index was released on the 16th December 2009. The most
recent file should also match ftp://ftp.1000genomes.ebi.ac.uk/vol1/ftp/sequence.index

There is also an associated statistics file e.g
	
 20120522.sequence.index.low_coverage.stats
 20120522.sequence.index.exome.stats

This contains summary statistics for each index using the base count data in column 25.

Please be aware that in the exome.stats files, the "COVERAGE" column refers to whole genome coverage, not coverage over targetted
regions. The exome coverage over targetted regions is better captured in *.HsMetrics.gz files under alignment_indices directory. 

The summaries it presents are

Withdrawn summary, how many files have been withdrawn for each given reason
Study id summary, how many basepairs and xcoverage of data per Study
Population summary, how many basepairs and xcoverage of data by Population
Center summary, how many basepairs and xcoverage of data by Submission Center, this is further broken down by study id
Sample summary, how many basepairs and xcoverage of data by Sample name

csv files containing statistics are also present in the form

20091216_201001025.stats.csv

This contain total numbers and differences between the two dated index files.

     Numbers of accessions
     Numbers of samples
     Numbers of samples with more than 4x coverage
     Number of gigbases per population
     Number of gigabases per platform
     Number of gigabases per submission center

These files will be generated for each new release of a sequence index 


Process of creating the sequence data collection in DCC

The DCC gets its sequence files from the SRA. Before publishing the files they under go some filtering so ensure good quality data is used.

These are the checks the DCC makes on the archive fastq files.

        Syntax Checks:

        -Each header line begins with @
        -The third line always starts with a +
        -There are four lines in each entry (implied by the above two rules)
        -On line3, if a name follows the + sign, the name has to match the one found in line1
        -The sequence and quality lines are the same length
        -For paired end files, the _1 and _2 files have the same number of reads in them. 
        -For SOLID colourspace fastq, each read starts with a base followed by a string of numbers

        Sequence Checks:

        -Read is longer than 35bp for Solexa, 25bp for Solid, and 30 bp for 454
        -Read does not contain any N's in the first 25, 30 or 35bp
        -Quality values are all 2 or higher in the first 25bp, 30bp or 35bp
        -The reads contain more than one type of base in the first 25, 30, or 35bp
        -Read does not contain more than 50% Ns in its whole length
	-Read does not contain characters other than ATGCN (this rule does not apply to SOLID reads)

The output files get the extension .filt.fastq.gz to indicate they have been filtered.

The DCC marks some files as withdrawn. These have a flag in column 21 of the  sequence.index. There are several reasons for this and 
they are given in column 23 of the sequence.index file. This is what those reasons mean.

	FAILED GENOTYPE QC, The fastq file was show to not be of the specified sample.

	FAILED ONE OF THE DCC PROCESSES, There were meta data consistency problems with the file or no reads in the file passed our qc checks.

	NOT YET AVAILABLE FROM ARCHIVE, The fastq files aren't yet available from the SRA

	SUPPRESSED IN ARCHIVE, The run has been suppressed by the submitter in the archive

 	EXCESSIVE SMALL INDELS, The run has an overly large number of small indels suggesting there was a bubble on the flow cell

        FAILED VERIFYBAM QC, The run failed its post alignment quality check for contaimination or sample swapping

	RELATED SAMPLE, The sample has been withdrawn due to being related to another sample in the project

The analysis.sequence.index has two additional failure statuses

        TOO LOW RAW COVERAGE, This means the sample has less than 8250000000 bases of raw sequence for a low coverage run and less than 1000000000 bases
        of raw sequence as an exome run and this means they can not possibly meet our completion critriea of at least 3x non duplicated aligned coverage for 
        low coverage and at least 70% of exome targets covered to 20x or more coverage in the exome so they are left out from our alignment processing

        TOO LOW ALIGNED COVERAGE, This means the sample has failed our aligned coverage criteria

