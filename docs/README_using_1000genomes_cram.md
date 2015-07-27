This readme is about using the CRAM files we distribute for our wgs alignments.

The 1000 Genomes project, from January 2015 will distribute alignments in cram format where possible.

CRAM is a referenced based compression format which allows storage of genome alignments in a space efficient manner

The CRAM spec itself is maintained as part of the htslib set of specifications. A [pdf of the CRAM 3.0 specification](http://samtools.github.io/hts-specs/CRAMv3.pdf) is available.

The command used to produce our CRAM files is 

CRAM files can be read natively by [samtools](https://github.com/samtools/samtools) and any other code based on [htslib](https://github.com/samtools/htslib). EMBL-EBI also provides a java toolkit - [cramtools](http://www.ebi.ac.uk/ena/software/cram-toolkit)

CRAM files don't contain the same level of sequence data as BAM files. CRAM files rely on the [cram reference registry](http://www.ebi.ac.uk/ena/software/cram-reference-registry) to provide the sequence data for its output. Depending on what CRAM reading tool you are using, there are different options for getting a local copy of the particular reference a cram file uses and pointing the tools to it.




