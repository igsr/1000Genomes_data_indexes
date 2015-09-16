#Data collections

###Collections

The International Genome Sample Resource (IGSR) provides access to data from **multiple** projects, including the **1000 Genomes Project**. As a consequence of this, data is organised in collections to reflect these different projects. Each of the directories in this directory contains data for a given collection. **Data from the 1000 Genomes projects can be found under 1000_genomes_project.**

###Collection layout

Within each collection directory, you will find information in README files, which describe the data and any processing which has been done.

In addition, index files provide a catalogue of the files available for that collection. The body of each index file is tab delimited. Index files also have a header section with lines starting with ##, providing information about the file, data and the columns. The column header is a line starting with a single # immediately before the body of the file. The sequence indices contain the locations of the sequence data in the European Nucleotide Archive (ENA).

The data directory in each collection directory is where the data is housed. Data is organised by population and then by sample, for example, 1000_genomes_project/data/YRI/NA19150/.

###Further information

If you are looking for a specific file, a list of all files on the site can be found in ftp://ftp.1000genomes.ebi.ac.uk/vol1/ftp/current.tree. Should you have any queries, please contact info@1000genomes.org.
