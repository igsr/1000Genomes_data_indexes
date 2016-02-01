## 2015-10-02 
## JL Rodriguez-Flores, jur2014@med.cornell.edu

## Included are genotypes in VCF format for 1000 Genomes Phase 3 mitochondrial DNAs
## Files were generated from rCRS reference and FASTA files of individual mtDNA sequences
## Code for the conversion is posted on GitHub (https://github.com/juansearch/callMom/)

## Input files
rCRS.fasta
1KG_No_Het.fasta (see below)

## Code
./callMom.v.0.2.py rCRS.fasta 1KG_No_Het.fasta 

callMom outputs a set of debugging files and the output VCF

## Output
ALL.chrMT.phase3_callmom.20130502.genotypes.vcf

## Post-processing
VCF was compressed using bgzip, indexed using tabix, and validated using vcf-validator

## update on 2016-02-01
The file 1KG_No_Het.fasta can be found at ftp://ftp.1000genomes.ebi.ac.uk/vol1/ftp/release/20130502/supporting/MT/1KG_No_Het.fasta.
