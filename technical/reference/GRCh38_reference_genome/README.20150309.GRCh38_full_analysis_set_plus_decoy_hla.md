#README for GRCh38_full_analysis_set_plus_decoy_hla reference

The reference fasta file in this directory was unpacked from Heng Li's [bwakit-0.7.12](http://sourceforge.net/projects/bio-bwa/files/bwakit/bwakit-0.7.12_x64-linux.tar.bz2/download).

Heng's fasta file is based on the [GRCh38 full analysis set](ftp://ftp.ncbi.nlm.nih.gov/genbank/genomes/Eukaryotes/vertebrates_mammals/Homo_sapiens/GRCh38/seqs_for_alignment_pipelines/GCA_000001405.15_GRCh38_full_plus_hs38d1_analysis_set.fna). The only addition is the HLA sequences.

This fasta file includes:

* Chromosomes: chr{chromosome number or name}

     e.g. chr1 or chrX
     chrM for the mitochondrial genome.

* Unlocalized scaffolds: chr{chromosome number or name}_{sequence_accession}v{sequence_version}_random

   e.g. chr17_GL000205v2_random

* Unplaced scaffolds: chrUn_{sequence_accession}v{sequence_version}

    e.g. chrUn_GL000220v1

* Alternate loci scaffolds: chr{chromosome number or name}_{sequence_accession}v{sequence_version}_alt

   e.g. chr6_GL000250v2_alt

* Patch scaffolds: chr{chromosome number or name}_{sequence_accession}v{sequence_version}_patch

   e.g. chrY_KN196487v1_patch 

* Decoy sequences: chrUn_{sequence_accession}v{sequence_version}_decoy

   e.g. chrUn_KN707606v1_decoy
   
* HLA sequences: HLA-{id}

   e.g. HLA-A*02:53N
 
This directory also contains some other files

[20150713_location_of_centromeres_and_other_regions.txt](ftp://ftp.1000genomes.ebi.ac.uk/vol1/ftp/technical/reference/GRCh38_reference_genome/20150713_location_of_centromeres_and_other_regions.txt) contains a list of all the modelled centromeric sequence     
that GRCh38 contains as well as the location of the PAR on chrY and the hetrochromatic sequence on chr7.

This was sourced from [ftp://ftp.ncbi.nlm.nih.gov/genomes/all/GCA_000001405.15_GRCh38/GCA_000001405.15_GRCh38_assembly_regions.txt](ftp://ftp.ncbi.nlm.nih.gov/genomes/all/GCA_000001405.15_GRCh38/GCA_000001405.15_GRCh38_assembly_regions.txt) on the 13th July

The [other_mapping_resources](ftp://ftp.1000genomes.ebi.ac.uk/vol1/ftp/technical/reference/GRCh38_reference_genome/other_mapping_resources) contains a set of vcf files for snps and indels for base quality recalibration
and indel realignment for GRCh38 mapping pipelines.

There is more explanation about these files in [ftp://ftp.1000genomes.ebi.ac.uk/vol1/ftp/technical/reference/GRCh38_reference_genome/other_mapping_resources/README.20150504.other_mapping_resources](ftp://ftp.1000genomes.ebi.ac.uk/vol1/ftp/technical/reference/GRCh38_reference_genome/other_mapping_resources/README.20150504.other_mapping_resources).
