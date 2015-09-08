#Illumina Platinum pedigree README

This README contains information relating to data associated with the Illumina Platinum pedigree data set, files for which are available under this directory.

###The Illumina Platinum pedigree data set

Information about the Platinum pedigree data is available from [Illumina](http://www.illumina.com/platinumgenomes/).

Illumina states:
>"If you have any questions, contact us at: platinumgenomes@illumina.com. Please note that while Platinum Genomes are freely available, Illumina does not offer technical support for these resources. Please cite this website and Illumina, Inc. in publications and other public usage of Platinum Genomes."

The pedigree sequence data was generated on the 17 member CEPH pedigree 1463 and includes a technical replicate of NA12882. The technical replicate is labelled as NA12882_2.

Additional description can be found in the [ENA entry for this project](http://www.ebi.ac.uk/ena/data/view/ERP001960).

###Alignment of the pedigree data to the reference genome

####Programs and reference data
The Illumina Platinum pedigree data was aligned to the reference genome using the following programs and reference datasets:

1. [BWA-MEM](https://github.com/lh3/bwa/blob/master/bwakit/README.md) [bwakit-0.7.12](http://sourceforge.net/projects/bio-bwa/files/bwakit/bwakit-0.7.12_x64-linux.tar.bz2/download)
2. [Samtools-1.2](http://www.htslib.org/doc/samtools-1.2.html)
3. [BioBamBam-0.0.191](https://github.com/gt1/biobambam/releases/tag/0.0.191-release-20150401083643)
4. [GATK-3.3-0](https://github.com/broadgsa/gatk-protected/tree/3.3)
5. [Cramtools-3.0](https://github.com/enasequence/cramtools/tree/cram3)
6. Indels for realignment 
   - Phase 3 biallelic indels from Shapeit2 lifted to GRCh38 using the NCBI remapper ftp://ftp.1000genomes.ebi.ac.uk/vol1/ftp/technical/reference/GRCh38_reference_genome/other_mapping_resources/ALL.wgs.1000G_phase3.GRCh38.ncbi_remapper.20150424.shapeit2_indels.vcf.gz
   - High quality, experiment-validated indel set Devine and Mills produced. The coordinates were lifted to GRCh38 by Alison Meynert from IGMM in Edinburgh using CrossMap and the UCSC chain files, filtered out ref == alt cases ftp://ftp.1000genomes.ebi.ac.uk/vol1/ftp/technical/reference/GRCh38_reference_genome/other_mapping_resources/Mills_and_1000G_gold_standard.indels.b38.primary_assembly.vcf.gz
7. SNPs for recalibration 
   - dbSNP 142 in GRCh38 coordinates ftp://ftp.1000genomes.ebi.ac.uk/vol1/ftp/technical/reference/GRCh38_reference_genome/other_mapping_resources/ALL_20141222.dbSNP142_human_GRCh38.snps.vcf.gz

####Reference genome: GRCh38 with alternative sequences, plus decoys and HLA

###Alignment availability

The alignments have been made available in CRAM format. Additional information on CRAM can be found in a dedicated README at the toplevel of this site. Further information is available at these locations:
- [http://www.1000genomes.org/faq/what-are-cram-files](http://www.1000genomes.org/faq/what-are-cram-files)
- [http://www.ebi.ac.uk/ena/software/cram-toolkit](http://www.ebi.ac.uk/ena/software/cram-toolkit)
- [http://www.ebi.ac.uk/ena/software/cram-usage](http://www.ebi.ac.uk/ena/software/cram-usage)
