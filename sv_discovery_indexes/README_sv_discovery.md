#SV discovery index files

The [sv_discovery_index directory of the 1000Genomes_data_indexes repo](https://github.com/igsr/1000Genomes_data_indexes/tree/master/sv_discovery_indexes) contains index files relating to the Human Genome Structural Variation Consortium. These index files point to the data stored in the European Nucleotide Archive (ENA)

Our README files are all written in markdown format, if the formatting makes these files difficult to read please email info@1000genomes.org and improve the files.

##smrt.sequence.index

This contains lists for the pacbio SMRT sequencing HDF5 files in the ENA. 

The columns in the file are 

1.  ENA FILE PATH, This is the FTP file path to the file on the ENA FTP site.
2.  MD5 CHECKSUM, This is an md5 checksum for the file.
3.  RUN ID, This is the ENA run accession for the file.
4.  STUDY ID, This is the ENA study accession for the file.
5.  STUDY NAME, This is the ENA study title for the file.
6.  CENTER NAME, This is the name of the CENTER who submitted the file.
7.  SUBMISSION ID, This is the ENA submission id for the file.
8.  SUBMISSION DATE, This is the date on which the file was submitted.
9.  SAMPLE ID, This is the ENA sample identifier for the file.
10. SAMPLE NAME, This is the coriell sample name for the file.
11. POPULATION, This is the population of the sample. For a full list of populations please see.
12. EXPERIMENT ID, This is the ENA experiment accession for the file.
13. INSTRUMENT PLATFORM, This is the sequencing platform the sample was sequenced using.
14. INSTRUMENT MODEL, THis is the model of sequencing machine used.
15. LIBRARY NAME, This is the submitter library name.
16. RUN NAME, This is the submission run name.
17. LIBRARY LAYOUT, This is the submitter library layout.
18. WITHDRAWN, This is a binary flag to indicate if the sample has been withdrawn.

##illumina_wgs.sequence.index

This contains lists for the Illumina wgs sequence fastq files in the ENA. 

The columns in the file are

1.  ENA FILE PATH, This is the FTP file path to the file on the ENA FTP site.
2.  MD5 CHECKSUM, This is an md5 checksum for the file.
3.  RUN ID, This is the ENA run accession for the file.
4.  STUDY ID, This is the ENA study accession for the file.
5.  STUDY NAME, This is the ENA study title for the file.
6.  CENTER NAME, This is the name of the CENTER who submitted the file.
7.  SUBMISSION ID, This is the ENA submission id for the file.
8.  SUBMISSION DATE, This is the date on which the file was submitted.
9.  SAMPLE ID, This is the ENA sample identifier for the file.
10. SAMPLE NAME, This is the coriell sample name for the file.
11. POPULATION, This is the population of the sample. For a full list of populations please see.
12. EXPERIMENT ID, This is the ENA experiment accession for the file.
13. INSTRUMENT PLATFORM, This is the sequencing platform the sample was sequenced using.
14. INSTRUMENT MODEL, THis is the model of sequencing machine used.
15. LIBRARY NAME, This is the submitter library name.
16. RUN NAME, This is the submission run name.
17. LIBRARY LAYOUT, This is the submitter library layout.
18. PAIRED_FASTQ, This is the other fastq file for this paired end run
19. ARCHIVE_READ_COUNT, The number of reads the ENA states is in the file
20. ARCHIVE_BASE_COUNT, The number of bases the ENA states is in the file

 
##Download instructions

Full instructions can be found on [http://www.ebi.ac.uk/ena/browse/read-download](http://www.ebi.ac.uk/ena/browse/read-download)

The URLS in the index files point to the ENA FTP site. The files can also be downloaded with aspera and globusgridftp

To download the file with aspera you need a command like

`ascp -i asperaweb_id_dsa.openssh -Tr -Q -l 100M -L- era-fasp@fasp.sra.ebi.ac.uk:vol1/ERA420/ERA420531/pacbio_hdf5/m150202_035116_42248_c100731862550000001823141405141532_s1_p0.1.bax.h5`

The asperaweb openssh key should come with standard ascp installs. 

In linux it is found under the aspera connect install directory .aspera/connect/etc/asperaweb_id_dsa.openssh 

The ENA globus endpoint is ebi#ena

##Reference datasets

The consortium has decided to align to the full GRCh38 reference genome include alts, decoy and HLA sequence. A full description of the reference data can be found in [README_sv_reference_datasets](https://github.com/igsr/1000Genomes_data_indexes/blob/master/sv_discovery_indexes/README_sv_reference_datasets.md)

##Data Reuse policy

The data produced by the Human Genome Structural Variation Consortium is released early in line with the Toronto Workshop pre publication data release guidelines. Our data reuse policy is described in (README_hgsvc_datareuse_statement.md)[https://github.com/igsr/1000Genomes_data_indexes/blob/master/sv_discovery_indexes/README_hgsvc_datareuse_statement.md].

Please email info@1000genomes.org to with any questions about either the index files or the data reuse policy
