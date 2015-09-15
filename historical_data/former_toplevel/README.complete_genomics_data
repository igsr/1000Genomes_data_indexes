Complete Genomics README NCBI, December 2012.

Samples analyzed by Complete Genomics are delivered to the project with a set of reports 
and analysis files distinct from those constructed by current Consortium workflows. 

Most of the files and md5s are listed in a series of index files found under ftp/complete_genomics_indices

This contains 4 files for the most recent release

20130820.cg_data.index  This lists the relative path and md5 for all the bam, vcf and supporting tar files available for 
                        this release of data
20130725.cg_data.untar.index This lists the contents and md5s of the untared supporting tar files  
20130815.cg_ref_data.index This lists the contents and md5s of all the ref directories
20130725.sra.index This lists the relative path at the SRA to get cSRA files which contain all the CG data from the NCBI

You need to associate the SRA relative paths with one of 3 root urls depending on how you want to get the data

http://ftp-trace.ncbi.nlm.nih.gov/sra/sra-instant/reads/ByRun/sra/SRR/
ftp://ftp-trace.ncbi.nlm.nih.gov/sra/sra-instant/reads/ByRun/sra/SRR/
fasp://anonftp@ftp-trace.ncbi.nlm.nih.gov/sra/sra-instant/reads/ByRun/sra/SRR/

The last url will use the aspera connect plugin which you can get from http://asperasoft.com/software/transfer-clients/connect-web-browser-plug-in/

The run level meta data is also found in the sequence index directory in a file called CompleteGenomics.sequence.index 


Each CG processed sample will provide the following data elements in the cannonical path 
/ftp/data/[sample ID]/cg_data/:

1. Alignment of evidence intervals to the reference in BAM format. Files are named "EvidenceOnly".
2. Alignment of evidence intervals to thereference with supporting reads in BAM format. 
   Files are named "EvidenceSupporting".
3. Integrated VCF file containing sample genotypes for SNPs, indels, structural variants, 
   and mobile insertion elements.
4. The Complete Genomics set of production reports as tarball. This is the entire ASM directory 
   less REF and EVIDENCE subdirectories.
5. The Complete Genomics REF directory

NOTES:
1. The full set of alignment data is available as SRA objects where reference chromosomes and bait 
sequences have been resolved to their appropriate NCBI accessions. This object includes all primary alignments 
of reads to reference and unaligned reads. It can be downloaded from SRA and extracted to SAM, FASTQ and other 
formats using the SRA toolkit. See http://www.ncbi.nlm.nih.gov/books/NBK47540/#SRA_Download_Guid_B.Reference_Compressio 
for further information on using the SRA toolkit to access alignment data in native reference compression(cSRA) format.
You can find the paths from the sra.index file above

2. Secondary mappings of reads with MAPQ=0, having more than 8 placements to the reference, are not preserved. 
The reads themselves are available in FASTA format from SRA via toolkit services (see Note 1).  Short sequence 
motifs with degenerate placement on the reference assembly are a partial manifestation of the reference genome 
itself hashed as 35-mers. To provide a comprehensive view of local complexity, the complete reference assembly 
has been hashed, and a table prepared with the following columns: 

1) chromosome accession.version, 
(2) position, 
(3) unique k-to-the-left, 
(4)unique k-to-the-right. 

Columns 3 and 4 thus provide the minimum sequence lengths required  to make postion (2) unique in the genome. 
This table is available at ftp://ftp.1000genomes.ebi.ac.uk/vol1/ftp/technical/reference/GRCh37_refseq.kmer.txt.gz 

3. The alignments are based on the reference sequence which can be found in ftp://ftp.1000genomes.ebi.ac.uk/vol1/ftp/technical/reference/cg_alignment_reference/

Supplemental reports and Ref files can also be found in the appropriate directories and as tarballs in the cg_data directories

The contents of these directories are

SUPPLEMENTAL REPORTS IN ASM 
According to the Complete Genomics FAQ, 
http://www.completegenomics.com/FAQs/Data-Results/, 

each genome analyzed by the Assembly Pipeline version 2.2 will include the following files in the ASM tarball:

SHORT NAME 	  FILE NAME(S)		CONTENT
Summary		  summary-* 		Summary statistics  file
				
                     			All called small variants, 
VCF		vcfBeta-*		CNVs, SVs, and MEIs in VCF

Variant	        var-*			All called small variants file

Master					All called small variants and
Variation	masterVarBeta-*		associated annotations
file

Gene		gene-*			Small variants in genes
annotations

dbSNP		dbSNPAnnotted-*		Sequence for all dbSNP sites
annotations

Gene		geneVarSummary-*	Summary of small variants in
summary		          		genes

Non-coding	ncRNA-*			All small variants in known
RNA annotations				microRNAs

CNV files	cnvSegmentsDiploidBeta-*,		Copy number segmentation
in ASM/CNV	cnvSegmentsNondiploidBeta-*,		results
		cnvDetailsDiploidBeta-*,
		cnvDetailsNondiploidBeta-*, and
		depthOfCoverage_100000-*

SV files	allJunctionsBeta-*,			All called junctions,
in ASM/SV	highConfidenceJunctionsBeta-*,	        structural variation events,
		evidenceJunctionDnbBeta-*, and	        and alignments of evidence
		evidenceJunctionClustersBeta-*,	        DNBs
		allSvEventsBeta-*,
		highConfidenceSvEventsBeta-*

MEI files	mobileElementInsertionsBeta-*,	        All called mobile element
in ASM/MEI	mobileElementInsertionsROCBeta-*,	insertion events detected
		mobileElementInsertionsRefCountsBeta-*

Reports in	circos-*, circosLegend-* coverage-*	Coverage quality 
ASM/		coverageByGcContent-*,			characteristics for whole
REPORTS	coverageCoding-*,				genome and exome, and
		coverageByGcContentCoding-*,		length distribuution of
		indelLength-*, and			called indels and
		substitutionLength-*			substitutions

We also extract the coverageRef files and put them in their own tarball and directory

Coverage and Reference Score	coverageRefScore-*	Values for each position in the reference genome by chromosome	

