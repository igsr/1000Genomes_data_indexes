#aCGH README

This file describes the aCGH data in this directory.

The file 1kg.aCGH.final.filtered.gc.corr.sorted.dat.20140814.gz is the processed raw probe-level aCGH data. It is a tabix-indexed data file with filtered and corrected probes in the following format,

        <chr> <start> <end> <Feature> <FeatureNum> <ProbeID> <GC%><Sample_1_logRatio> <Sample_2_logRatio> .........<Sample_2534_logRatio>
 
acghSampleID.txt gives the sampleID'sfor the corresponding columns in the aCGH data file.

In addition, the submitted data has accession [GSE70188](http://www.ncbi.nlm.nih.gov/bioproject/?term=GSE70188).

