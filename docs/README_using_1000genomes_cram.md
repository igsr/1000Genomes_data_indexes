#IGSR CRAM Tutorial

From the first release of GRCh38 alignments onwards, we are releasing our alignment files in cram format.  

As htslib and picard can read CRAM files natively now most standard tools should be able to read these files natively. 

Here are details about how to view cram files, convert from cram to bam, how we produced the cram files and the CRAM specification.

1. Using CRAM files

CRAM Files can be read by both samtools and picard. EMBL-EBI also provides a java called cramtools (http://www.ebi.ac.uk/ena/software/cram-toolkit)

- Reading a CRAM file with samtools - standard samtools view commands work with CRAM files. You must be using samtools v1.2 or higher for this to work

`>samtools view $input.cram -h chr22:1000000-1500000 | less`

- Converting a CRAM file to a BAM File - some tools still need BAM files rather than CRAM you can convert from CRAM to BAM easily

`>java -jar cramtools-3.0.jar bam  -I $input.cram -R $reference.fa -O $output.bam`

Please note the first time you run these commands the program reading the CRAM file must download the reference sequence data

2. The CRAM reference registry

Because CRAM does not contain the same level of sequence data as BAM files, it relies on the CRAM reference registry to provide reference sequences for CRAM to output uncompressed sequences.  The reference must be available at all times. Losing it may be equivalent to losing all your read sequences. Retrieval of reference data from the registry is supported by using MD5 or SHA1 checksums using the following URLs:

`www.ebi.ac.uk/ena/cram/md5/<hashvalue>`
`www.ebi.ac.uk/ena/cram/sha1/<hashvalue>`

The md5 values for all GRCh38 reference chromosomes and contigs are included in CRAM file headers. These are mandatory fields in the CRAM specification.  

When first loading data from our CRAM files, both cramtools and htslib will download sequence from the cache. You can speed up the initial retrieval by preparing the cache in advance.

1. Download the reference file from our FTP site. ftp://ftp.1000genomes.ebi.ac.uk/vol1/ftp/technical/reference/GRCh38_reference_genome/GRCh38_full_analysis_set_plus_decoy_hla.fa
2. Run the seq_cache_populate.pl script from samtools - http://www.htslib.org/workflow/#mapping_to_cram  
`>perl samtools/misc/seq_cache_populate.pl -root /path/to/cache /path/to/GRCh38_full_analysis_set_plus_decoy_hla.fa`
3. Set the cache environment variables 
`>export REF_PATH=/path/to/cache/%2s/%2s/%s:http://www.ebi.ac.uk/ena/cram/md5/%s`
`>export REF_CACHE=/path/to/cache/%2s/%2s/%s`

1. What is CRAM format

CRAM is a reference-based compression of sequence data. The compression only works for resequencing data for organisms that have well-established reference genomes.  After aligning sequences reads to a reference genome, rather than storing every base pair of a sequence read, the approach stores only the difference between the read and the reference, hence reducing the space needed for storing sequence reads.  Additional compression can be archived in lossy mode by controlled loss of quality information and unaligned reads, by dropping read names and other information. The level of compression can be fine tuned based on users' experiment design. With associated tools, the compressed data can be seamlessly uncompressed and fed into downstream analysis.

This compression method was first developed by Ewan Birney's group in European Bioinformatics Institute (EBI) (Hsi-Yang Fritz, et al. (2011). Genome Res. 21:734-740).  Now the CRAM format and toolkits associated with it are used routinely by European Nucleotide Archive http://www.ebi.ac.uk/ena/software/cram-toolkit

2.  How did we compress the GRCh38 BAMs to CRAMs

This section lists the softwares and detailed parameters used for compressing the GRCh38 BAMs into CRAM. The CRAM version we used is CRAM 3.0, which includes the file format specification and associated toolkit CRAMtools:

CRAM specification: http://samtools.github.io/hts-specs/CRAMv3.pdf
CRAMtools: https://github.com/enasequence/cramtools/releases/tag/v3.0

Lossy mode compression was used to convert all the GRCh38 BAMs to CRAMs. All quality scores were binned based on Illumina 8-binning scheme.  Below is the command line used for the compression:

`>java -jar cramtools-3.0.jar cram --ignore-tags OQ:CQ:BQ --capture-all-tags --lossy-quality-score-spec '*8' --preserve-read-names -O $output.cram -R GRCh38_full_analysis_set_plus_decoy_hla.fa -I $input.bam`

3.  What is CRAM reference registry

Because CRAM does not contain the same level of sequence data as BAM files, it replies on the CRAM reference registry to provide reference sequences for CRAM to output uncompressed sequences.  The reference must be available at all times. Losing it may be equivalent to losing all your read sequences. Retrieval of reference data from the registry is supported by using MD5 or SHA1 checksums using the following URLs:

`www.ebi.ac.uk/ena/cram/md5/<hashvalue>`
`www.ebi.ac.uk/ena/cram/sha1/<hashvalue>`

The md5 values for all GRCh38 reference chromosomes and contigs are included in CRAM file headers. These are mandatory fields in the CRAM specification.  

When first loading data from our CRAM files, both cramtools and htslib will download sequence from the cache. You can speed up the initial retrieval by preparing the cache in advance.

1. Download the reference file from our FTP site. ftp://ftp.1000genomes.ebi.ac.uk/vol1/ftp/technical/reference/GRCh38_reference_genome/GRCh38_full_analysis_set_plus_decoy_hla.fa
2. Run the seq_cache_populate.pl script from samtools - http://www.htslib.org/workflow/#mapping_to_cram  
`>perl samtools/misc/seq_cache_populate.pl -root /path/to/cache /path/to/GRCh38_full_analysis_set_plus_decoy_hla.fa`
3. Set the cache environment variables 
`>export REF_PATH=/path/to/cache/%2s/%2s/%s:http://www.ebi.ac.uk/ena/cram/md5/%s`
`>export REF_CACHE=/path/to/cache/%2s/%2s/%s`

4.  How to use a CRAM file

CRAM as a framework technology includes not only the file format but also an array of java-based tools that work with the format.  Widely used third party tools such as Samtools and tools that reply on Samtools htslib and Picard/HTSJDK also introduced new functionalities to interface with CRAM.  You can basically use these tools natively pretty much the same way as you use them for BAMs. Here we listed a few example commands:

To view chr22:1000000-1500000 of a cram file using Samtools (version 1.2 or higher):
`>samtools view $input.cram -h chr22:1000000-1500000 | less`

To convert a cram file to a BAM file using cram tools:
`>java -jar cramtools-3.0.jar bam  -I $input.cram -R $reference.fa -O $output.bam`



This readme is about using the CRAM files we distribute for our wgs alignments.

The 1000 Genomes project, from January 2015 will distribute alignments in cram format where possible.

CRAM is a referenced based compression format which allows storage of genome alignments in a space efficient manner

The CRAM spec itself is maintained as part of the htslib set of specifications. A [pdf of the CRAM 3.0 specification](http://samtools.github.io/hts-specs/CRAMv3.pdf) is available.

The command used to produce our CRAM files is 

CRAM files can be read natively by [samtools](https://github.com/samtools/samtools) and any other code based on [htslib](https://github.com/samtools/htslib). EMBL-EBI also provides a java toolkit - [cramtools](http://www.ebi.ac.uk/ena/software/cram-toolkit)

CRAM files don't contain the same level of sequence data as BAM files. CRAM files rely on the [cram reference registry](http://www.ebi.ac.uk/ena/software/cram-reference-registry) to provide the sequence data for its output.

The first time a CRAM file is read, the sequence data must be retrieved from the reference registry and stored locally. This can slowdown the retrieval of data.

You can expedite this by creating a copy of the reference cache before trying to read the files. (Samtools provided a method to do this](http://www.htslib.org/workflow/#mapping_to_cram).

Download our [reference fasta file](ftp://ftp.1000genomes.ebi.ac.uk/vol1/ftp/technical/reference/GRCh38_reference_genome/GRCh38_full_analysis_set_plus_decoy_hla.fa)

Run [samtools/misc/seq_cache_populate.pl](https://github.com/samtools/samtools/blob/develop/misc/seq_cache_populate.pl) -root /path/to/cache GRCh38_full_analysis_set_plus_decoy_hla.fa

Then set these two environment variables

export REF_PATH=/path/to/cache/%2s/%2s/%s:http://www.ebi.ac.uk/ena/cram/md5/%s  
export REF_CACHE=/path/to/cache/%2s/%2s/%s

This process will work with both samtools and cramtools.
