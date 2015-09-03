#IGSR CRAM Tutorial

From the first release of GRCh38 alignments onwards, we are releasing our alignment files in CRAM format. CRAM is a reference-based compression of sequence data. 

Both htslib and picard can read CRAM files, many standard tools should be able to read these files natively. 

Here are details about how to view CRAM files, convert from CRAM to BAM, how we produced the cram files and the CRAM specification.

##Using CRAM files

CRAM Files can be read by both samtools and picard. EMBL-EBI also provides a java API called cramtools (http://www.ebi.ac.uk/ena/software/cram-toolkit)

- Reading a CRAM file with samtools - samtools view commands work with CRAM files. This functionality needs samtools v1.2 or higher

`>samtools view $input.cram -h chr22:1000000-1500000 | less`

- Converting a CRAM file to a BAM File - some tools still need BAM files rather than CRAM. You can convert from CRAM to BAM easily

`>java -jar cramtools-3.0.jar bam  -I $input.cram -R $reference.fa -O $output.bam`

Please note the first time you run these commands the program reading the CRAM file must download the reference sequence data from an online cache. This process can be speeded up if you download the required reference file and build a local copy of the cache in advance. This process is described below in the CRAM reference registry section.

##The CRAM reference registry

Because CRAM does not contain the same level of sequence data as BAM files, it relies on the CRAM reference registry to provide reference sequences for CRAM to output uncompressed sequences.  The reference must be available at all times. Losing it is equivalent to losing all your read sequences. Retrieval of reference data from the registry is supported by using MD5 or SHA1 checksums using the following URLs:

`www.ebi.ac.uk/ena/cram/md5/<hashvalue>`
`www.ebi.ac.uk/ena/cram/sha1/<hashvalue>`

The md5 values for all GRCh38 reference chromosomes and contigs are included in CRAM file headers. These are mandatory fields in the CRAM specification.  

The following process can be used to download and prepare the cache in advance to speed up initial reads of the sequence data from CRAM files. 

1. Download the reference file from our FTP site. ftp://ftp.1000genomes.ebi.ac.uk/vol1/ftp/technical/reference/GRCh38_reference_genome/GRCh38_full_analysis_set_plus_decoy_hla.fa
2. Run the seq_cache_populate.pl script from samtools - http://www.htslib.org/workflow/#mapping_to_cram  
`>perl samtools/misc/seq_cache_populate.pl -root /path/to/cache /path/to/GRCh38_full_analysis_set_plus_decoy_hla.fa`
3. Set the cache environment variables 
`>export REF_PATH=/path/to/cache/%2s/%2s/%s:http://www.ebi.ac.uk/ena/cram/md5/%s`
`>export REF_CACHE=/path/to/cache/%2s/%2s/%s`

##How did we compress the BAMs to CRAMs

We use cramtools to convert the BAM files our alignment pipeline produces to CRAM Files. The files are compressed using a lossy mode, this bins all quality stores into the 8-binning scheme defined by Illumina.

`>java -jar cramtools-3.0.jar cram --ignore-tags OQ:CQ:BQ --capture-all-tags --lossy-quality-score-spec '*8' --preserve-read-names -O $output.cram -R GRCh38_full_analysis_set_plus_decoy_hla.fa -I $input.bam`

##More information about CRAM format.

As mentioned above CRAM represents a reference-based compression of sequence data. After aligning sequences reads to a reference genome, rather than storing every base pair of a sequence read, the approach stores only the difference between the read and the reference, hence reducing the space needed for storing sequence reads.  Additional compression can be archived in lossy mode by controlled loss of quality information and unaligned reads, by dropping read names and other information. The level of compression can be fine tuned based on users' experiment design. With associated tools, the compressed data can be seamlessly uncompressed and fed into downstream analysis.

This compression method was first developed by Ewan Birney's group in European Bioinformatics Institute (EBI) (Hsi-Yang Fritz, et al. (2011). Genome Res. 21:734-740).  The specification itself is maintained by the HTSlib group alongside the BAM, VCF and BCF specifications http://samtools.github.io/hts-specs/CRAMv3.pdf.

If you have any questions about our CRAM files or our alignment pipeline, please email info@1000genomes.org

