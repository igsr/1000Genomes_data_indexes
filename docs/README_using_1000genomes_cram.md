#IGSR CRAM Tutorial
##2015-08-06

As the throughput of DNA sequencing increases rapidly, the cost of storing the sequencing data is becoming more of a concern.  In the interest of reducing data volume, the GRCh38 alignment data of all 1000 Genomes Project reads are compressed and released to the community in CRAM format.  As this is the first large scale data release in CRAM format, we provide this README for our users to get familiar with the new format and tools associated with the format.  This README covers the following topics:

1.  What is CRAM format
2.  How did we compress the GRCh38 BAMs to CRAMs
3.  What is CRAM reference registry
4.  How to use a CRAM file

1. What is CRAM format

CRAM is a reference-based compression of sequence data. The compression only works for resequencing data for organisms that have well-established reference genomes.  After aligning sequences reads to a reference genome, rather than storing every base pair of a sequence read, the approach stores only the difference between the read and the reference, hence reducing the space needed for storing sequence reads.  Additional compression can be archived in lossy mode by controlled loss of quality information and unaligned reads, by dropping read names and other information. The level of compression can be fine tuned based on users' experiment design. With associated tools, the compressed data can be seamlessly uncompressed and fed into downstream analysis.

This compression method was first developed by Ewan Birney's group in European Bioinformatics Institute (EBI) (Hsi-Yang Fritz, et al. (2011). Genome Res. 21:734-740).  Now the CRAM format and toolkits associated with it are used routinely by European Nucleotide Archive (http://www.ebi.ac.uk/ena/software/cram-toolkit).

2.  How did we compress the GRCh38 BAMs to CRAMs

This section lists the softwares and detailed parameters used for compressing the GRCh38 BAMs into CRAM. The CRAM version we used is CRAM 3.0, which includes the file format specification and associated toolkit CRAMtools: 

CRAM specification: http://samtools.github.io/hts-specs/CRAMv3.pdf
CRAMtools: https://github.com/enasequence/cramtools/releases/tag/v3.0

Lossy mode compression was used to convert all the GRCh38 BAMs to CRAMs. All quality scores were binned based on Illumina 8-binning scheme.  Below is the command line used for the compression:

`>java -jar cramtools-3.0.jar cram --ignore-tags OQ:CQ:BQ --capture-all-tags --lossy-quality-score-spec '*8' --preserve-read-names -O $output.cram -R GRCh38_full_analysis_set_plus_decoy_hla.fa -I $input.bam`

3.  What is CRAM reference registry

Because CRAM does not contain the same level of sequence data as BAM files, it replies on the CRAM reference registry to provide reference sequences for CRAM to output uncompressed sequences.  The reference must be available at all times. Losing it may be equivalent to losing all your read sequences. Retrieval of reference data from the registry is supported by using MD5 or SHA1 checksums using the following URLs:
www.ebi.ac.uk/ena/cram/md5/<hashvalue>
www.ebi.ac.uk/ena/cram/sha1/<hashvalue>

The md5 values for all GRCh38 reference chromosomes and contigs are included in CRAM file headers as they are mandatory by CRAM specification.  Retrieval of reference sequence from local cache can speedup reading CRAMs at the first instance (see below for details).

4.  How to use a CRAM file

CRAM as a framework technology includes not only the file format but also an array of java-based tools that work with the format.  Widely used third party tools such as Samtools and tools that reply on Samtools htslib and Picard/HTSJDK also introduced new functionalities to interface with CRAM.  You can basically use these tools natively pretty much the same way as you use them for BAMs. Here we listed a few example commands:

To view chr22:1000000-1500000 of a cram file using Samtools (version 1.2 or higher):
`>samtools view $input.cram -h chr22:1000000-1500000 | less`

To convert a cram file to a BAM file using cram tools:
`>java -jar cramtools-3.0.jar bam  -I $input.cram -R $reference.fa -O $output.bam`

The first time when a CRAM file is read, it is going to be much slower than when an equivalent BAM is read.  CRAM differs from BAM in that it is reference-based compression with (typically) an externally-stored reference.  Those externally-stored references have to come from somewhere, and if you don't already have them locally then that is going to take some time to download.  You can expedite this by creating a local copy of the reference cache before trying to read the CRAM files.  It is always a good idea to share a communal copy of the reference within the group/institute.  Samtools provides some tools to help with this (seq_cache_populate.pl, $REF_PATH, $REF_CACHE); please see tutorial information at http://www.htslib.org/workflow/#mapping_to_cram.  

Here is a simple step by step instruction:
	1) Download GRCh38 reference fasta file GRCh38_full_analysis_set_plus_decoy_hla.fa from 
ftp://ftp.1000genomes.ebi.ac.uk/vol1/ftp/technical/reference/GRCh38_reference_genome/GRCh38_full_analysis_set_plus_decoy_hla.fa
	2) Run seq_cache_populate.pl provided by Samtools to covert the downloaded reference fasta file to a directory tree of reference sequence MD5 sums. MD5 sum of the each reference sequence is used as the key to link a CRAM file to the reference genome used to generate it. 
>perl samtools/misc/seq_cache_populate.pl -root /path/to/cache /path/to/GRCh38_full_analysis_set_plus_decoy_hla.fa
	3) Set these two environment variables.  By default Samtools and Cramtools check the reference MD5 sums (@SQ “M5” auxiliary tag) in the directory pointed to by $REF_PATH environment variable, falling back to querying the EBI reference genome server, and further falling back to the @SQ “UR” field if these are not found
>export REF_PATH=/path/to/cache/%2s/%2s/%s:http://www.ebi.ac.uk/ena/cram/md5/%s >export REF_CACHE=/path/to/cache/%2s/%2s/%s
This process works with both samtools and cramtools.



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
