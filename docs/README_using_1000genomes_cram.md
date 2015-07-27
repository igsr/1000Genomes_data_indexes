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
