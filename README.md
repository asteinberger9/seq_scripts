## asteinberger9_seqscripts

### This repo contains scripts written by Andrew Steinberger.

Please verify accuracy of outputs. These scripts have been repeatedly tested for accuracy on multiple sequencing data sets, however there is no guarantee they will do so for every data set out there. If issues are found please contact me and I will work on fixing them as I am able.

If you use these scripts for a publication please cite them:
[![DOI](https://zenodo.org/badge/82193080.svg)](https://zenodo.org/badge/latestdoi/82193080)

### simper_pretty.R
This script is meant to rapidly perform the simper function from the R package [vegan](https://cran.r-project.org/web/packages/vegan/index.html) for all comparisons of interest in a dataset. Inputs are OTU and metadata tables, and the output is a .csv. User can tailor contents of .csv by setting perc_cutoff, low_cutoff, and low_val. This function can also handle taxonomic levels instead of OTU, but currently only select formats are compatable. Requires installation of the R package 'vegan'.

#### Inputs:
x: OTU table  
metrics: metadata table  
interesting: a list of the column headers for the columns of interest in the metrics file. e.g. c('int1','int2','int3')  
perc_cutoff: % cutoff for output OTUs, as decimal (i.e. write 50% as 0.5), larger % increases number OTUs in output.  
low_cutoff: 'y' if want to REMOVE OTUs that contribute less than 1%  
low_val: set value of low cutoff (0.01), ignored if low_cutoff='n'.  
output_name: the name that is appended to the output filename "_clean_simper.csv".  

### R_krusk.R
This script takes the output .csv of simper_pretty.R, and the OTU/metadata/taxonomy tables, and performs the non-parametric Kruskal-Wallis rank-sum test on each otu in the .csv file. Output is a .csv file containing the same contents of simper.pretty output with the following info: p-value, fdr corrected p-value, otu taxonomic classification (if applicable), mean rel. abund and std dev of otu/tax_lvl in group 1 of comparison, and mean rel. abund and std dev of otu/tax_lvl in group 2 of comparison. Requires installation of R packages 'vegan' and 'dplyr'.

#### Inputs:
otu: OTU table  
metrics: metadata table  
taxonomy: The .taxonomy file output from classify.otu command in [mothur](https://www.mothur.org) (optional)  
csv: output from simper.pretty, must be imported as data.frame. e.g. csv= data.frame(read.csv("PATH to name_clean_simper.csv"))  
interesting: a list of the column headers for the columns of interest in the metrics file, should be same as simper.pretty inputs. e.g. c('int1','int2','int3')  
output_name= the name that is appended to the output filename "_krusk_simper.csv".  

### Examples of Recommended Input Formats

  While the script does have some flexibility with data input format, it was designed and optimized to work with formats detailed below. For best results make sure there are no spaces or _ in the otu table column headers, nor the Metadata column headers and values. Also be sure that sample names match across OTU and metadata tables and OTU names match between OTU and taxonomy tables. OTU names can be taxonomic rankings (Phylum/Family/Genus), however if simper.pretty returns sample names rather than OTU names in the OTU column, try to transpose your OTU table prior to running simper.pretty. Ex. transp.otu_table <- as.data.frame(t(otu_table))

#### OTU Table

|               | OTU001        | OTU002  | OTU003|
| ------------- |:-------------:|:-------:|------:|
| Sample 1      |       26      |    99   |   33  |
| Sample 2      |       12      |    10   |    0  |
| Sample 3      |       81      |     8   |  104  |

#### Metadata Table

|               | Interesting1  | Int.Var.2| Int.Var.3|
| ------------- |:-------------:|:--------:|---------:|
| Sample 1      |       A       |    Ant   |   Rep1   |
| Sample 2      |       B       |    Soil  |   Rep1   |
| Sample 3      |       C       |    Ant   |   Rep2   |

#### Taxonomy Table (optional, stright from mothur output)

|               | Size  | Taxonomy                                                                                             |
| ------------- |:-----:|:----------------------------------------------------------------------------------------------------:|
| OTU001        | 99256 |Bacteria;Proteobacteria;Gammaproteobacteria;Pseudomonadales;Moraxellaceae;Acinetobacter;              |
| OTU002        | 48594 |Bacteria;Proteobacteria;Gammaproteobacteria;Pseudomonadales;Pseudomonadaceae;Pseudomonas;             |
| OTU003        | 41059 |Bacteria;Firmicutes;Bacilli;Lactobacillales;Lactobacillales_unclassified;Lactobacillales_unclassified;|
