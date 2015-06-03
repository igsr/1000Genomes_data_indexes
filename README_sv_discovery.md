#SV discovery index files

This directory contains index files relating to the 1000 Genomes SV discovery project. These index files point to the data stored in the European Nucleotide Archive (ENA)

[smrt.sequence.index](#smrt.sequence.index)  
[illumina_wgs.sequence.index](#illumina_wgs.sequence.index)  
[Download instructions](#Download instructions)  
[Data Reuse Policy](#Data Reuse policy)    

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

Full instructions can be found on [http://www.ebi.ac.uk/ena/browse/read-download]()http://www.ebi.ac.uk/ena/browse/read-download)

The URLS in the index files point to the ENA FTP site. The files can also be downloaded with aspera and globusgridftp

To download the file with aspera you need a command like

`ascp -i asperaweb_id_dsa.openssh -Tr -Q -l 100M -L- era-fasp@fasp.sra.ebi.ac.uk:vol1/ERA420/ERA420531/pacbio_hdf5/m150202_035116_42248_c100731862550000001823141405141532_s1_p0.1.bax.h5`

The asperaweb openssh key should come with standard ascp installs. 

In linux it is found under the aspera connect install directory .aspera/connect/etc/asperaweb_id_dsa.openssh 

The ENA globus endpoint is ebi#ena

##Data Reuse policy

This directory contains all the data associated with the 1000 Genomes SV group NHGRI funded
project to create a high quality catalog of structural variation using various sequencing
approaches.

All data in this directory is part of that project.

Continuing the philosophy of the 1000 Genomes Project, the data producers for this Project 
will release the Project data quickly, prior to publication, in the expectation that they 
will be valuable for many researchers. In keeping with Fort Lauderdale principles, data 
users may use the data for many studies, but are expected to allow the data producers 
to make the first presentations and to publish the first paper with global analyses of the data.

###Global analyses of Project data

The Project plans to publish global analyses of the sequence data and quality, structural variants, 
STRs and microsatellites and haplotypes and LD patterns, population genetic phenomena such as 
population comparisons, mutation rates, and signals of selection, and functional annotations, 
as well as analyses of regions of general interest such as HLA. Talks, posters, and papers on 
all such analyses are to be published first by the 1000 Genomes SV group, by approved presenters
on behalf of the Project, with the group as author. When the first major Project paper on these 
analyses is published, then researchers inside and outside the Project are free to present and 
publish using the Project data for these and other analyses.

###Large-scale analyses of Project data

Groups within the Project may make presentations and publish papers on more extensive analyses of 
topics to be included in the main analysis presentations and papers, coincident with the main 
Project analysis presentations and papers. The major points would be included in the main Project 
presentations and papers, but these additional presentations and papers allow more focused discussion 
of methods and results. The author list would include the Consortium.

###Methods development using Project data

Researchers who have used small amounts of Project data (<= one chromosome in a region not of general interest) 
may present methods development posters, talks, and papers that include these data prior to the first major 
Project paper, without needing Project approval or authorship, although the Project should be acknowledged. 
Methods presentations or papers on global analyses or analyses using large amounts of Project data, on 
topics that the Consortium plans to examine, would be similar to large-scale analyses of Project data: 
researchers within the Project may make presentations or submit papers at the same time as the main 
Project presentations and papers, and others could do so after the Project publishes the first major analysis paper.

###Disease studies using Project data

Researchers may present and publish on use of Project data in specific chromosome regions (that are not of general interest) 
or as summaries (such as the total number of variants) for disease studies without Project approval, prior to 
the first major Project paper being published. The Project should not be listed as an author.

###Population comparisons using Project data

Researchers may use Project data as controls or additional information for comparisons with their samples 
from people with particular diseases or from other populations, prior to the major Project paper being 
published, as long as the analyses that the Project plans to do are not included. These are not Project 
studies, and the Project should not be listed as an author.

Researchers who have questions about whether they may make presentations or submit papers using Project 
data, or whether to include the 1000 Genomes SV group as an author on any paper can contact us.

Please email info@1000genomes.org to enquire.