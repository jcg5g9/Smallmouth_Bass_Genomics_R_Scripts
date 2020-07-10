# Smallmouth_Bass_Genomics_R_Scripts

# Aim 1 - Preliminary Data Summary
### File: SMB_ALLELE_FREQUENCIES.Rmd

Purpose: For Aim 1, I am assessing sample quality, mean read depth per contig, folded minor allele frequency distributions, and allele and genotype frequencies. All allele frequency and genotype frequency distributions are calculated after removal of samples with greater than 20% missing data (3 total samples), poor phred-quality score (less than 20), and one SNP per contig. All distributions are then calculated pre-filtering for missing data across SNPs, minor allele count (mac = 2, maf = 0.011), and greater than 45% heterozygosity; and after all filtering.

### Genomic Data used:
#### Before removal of poor quality samples: 127,428 SNPs, 95 samples
#### Pre-filtered dataset (after removing poor quality samples, phred score less than 20), one SNP per contig: 65,254 SNPs, 92 samples
#### Post-filtered dataset (after all filters): 50,828 SNPs, 92 samples

# Aim 2 - Admixture Analysis
### File: SMB_ADMIXTURE.Rmd

Purpose: Here I assess population genomic structure both between Smallmouth Bass and Spotted Bass and with Smallmouth Bass subspecies (Northern Smallmouth Bass and Neosho Smallmouth Bass). My aims are 1) to first screen for first generation or later-generation interspecies hybrids of Smallmouth Bass and Spotted Bass, 2) to determine if population structure patterns differ when including or excluding any hybrids detected in aim (1), and 3) to assess hierarchical population structure among the Smallmouth Bass subspecies. With the analysis, I will infer patterns of genomic differentiation and admixture between populations. I will match results from ADMIXTURE to results from SNPhylo, which will produce phylognetic tree to show evolutionary relationships among individuals.

### Genomic Data used: 92 samples (64 Neosho Smallmouth Bass, 24 Northern Smallmouth Bass, 4 Spotted Bass), 50,828 SNPs. 

# Aim 3 - Coancestry Analysis with FINERADSTRUCTURE
### File: SMB_FINERADSTRUCTURE.Rmd

Purpose: Here I assess fine-scale population structure among all samples (both including and excluding the hybrid discovered in ADMIXTURE analysis (See SMB_ADMIXUTRE.Rmd)) using nearest-neighbor haplotype linkage in the program fineRADstructure, which is a derivation of the program fineSTRUCTURE specifically for RAD data. With this analysis, I want to determine 1) coancestry among individuals in my dataset by producing a coancestry matrix. This analysis allows one to see which samples are most closely related and can be used to determine which samples belong to which populations. Samples with more coancestry (forming coancestry blocks) can be considered part of the same population. Also with this population, I aim to determine 2) the connectivity among individuals and among populations by interpreting coancestry between samples belonging to putatively different populations.
 
### Genomic Data used:

#### filtered dataset (excluding the hybrid (BFC10) and not including one rad per rad tag: 98,659 SNPs, 91 samples
#### fineRADstructure requires multiple SNPs per rad-tag to increase statistical power, because it is comparing samples based on haplotype information. It is creating pseudo-haplotypes from non-phased SNP data (in other words, since missing nucleotides cannot be imputed without a reference genome, individual SNPs on the same rad-tag are strung together to create a haplotype for analysis)

# Aim 4 - Outlier SNP Analysis
### File: SMB_BAYESCAN.Rmd

Purpose: The Bayescan Analysis section includes all data reading, manipulation, organization, analysis, and plotting used to interpret output from the program Bayescan. Bayescan is a bayesian computation program that reads SNP data (in the form of a Bayescan file, which is converted from a VCF to a Bayescan format in the coverter program PGDspider) and calculates average pairwise Fst values among a priori designated populations at each individual SNP site. The program then determines whether any of the individual SNPs produce outlier Fst values (extremely high Fst outliers or extremely ow Fst outliers), which are considered likely indicative of sites under natural selection.

The PCAdapt Analysis section includes all data reading, manipulation, organization, analysis, and plotting used to run the R package PCAdapt. PCAdapt is an R program that identifies outlier Fsts (similar to Bayescan) using a multivariate analysis approach (PCA). Significant, outlier SNPs are those that are identified as loading very heavily on the principal components accounting for the highest amount of genomic variation in the data.

### Genomic Date used:

#### Fully filtered dataset, excluding hybrids (BFC10) and excluding Spotted Bass: 50,828 SNPs, 87 samples

# Aim 5 - Local Adaptation Analysis with DAPC
### File: SMB_DAPC.Rmd

Purpose: Here I am using Discriminant Analysis of Principal Components (DAPC), which is a multivariate method that performs discriminant analysis on the principal components with the highest proportion of variance from principal component analysis. The benefit of DAPC is that it minimizes within group variance while maximizing among-group variance, so it can be used to assess broad-scale population structure in genomic data. In this analysis, I am 1) conducting DAPC on outlier SNPs detected by two separate analyses (PCAdapt and BayeScan) in order to see how populations are organized based on only these outlier SNPs. Then, I am 2) conducting DAPC on neutral SNPs to see if the population structure is different for neutral SNPs.

In PCAdaapt, All individual SNPs in the original dataset (50,828) are assessed individually as to their regression on the first two pricipal components (the principal components accounting for the highest amount of genomic variance) from a Principal Component Analysis. The Principal Component Analysis decomposes the dataset into groups that represent population genomic structure. Then, SNPs that are significantly (after Bonferroni correction) correlated with the first two PCs are considered outlier SNPs and are presumed to be under selection, driving the population structure that is obtained by the Principal Component Analyssi. 

In BayeScan, Fst values are calculated for each individual SNP in the original dataset (50,828 SNPs) over an a priori designatied set of populations (populations must be known for BayeScan to run, because the program requires population as input). SNPs with Fsts that deviate from neutrality given the overall Fsts calculated among populations are considered to be under natural selection and have a Log of Posterior Odds value at or above 1.5.

For BayesCan, I used the primary population structure obtained from PCAdapt to determine whether SNPs would be run at the level of taxon (Spotted Bass, Neosho Smallmouth Bass, Northern Smallmouth Bass), or populations (all populations obtained from ADMITURE and fineRADstructure)

### Genomic Data used:
      
#### Fully-filtered SMB dataset, Outlier SNPs (outliers shared between Bayescan and PCAdapt): 131 SNPs, 87 Samples
#### Fully-filtered SMB dataset, Neutral SNPs (neutrals shared between Bayescan and PCAdapt): 41,236 SNPs, 87 Samples
#### Fully-filtered Neosho SMB dataset, Outlier SNPs (outliers shared between Bayescan and PCAdapt): 29 SNPs, 63 Samples
#### Fully-filtered Neosho SMB dataset, Neutral SNPs (neutrals shared between Bayescan and PCAdapt): 35,025 SNPs, 63 Samples

# Aim 6 - Map Building
### File: SMB_MAPS.Rmd

Purpose: Here I am generating a map for easily displaying my sample distribution geographically.

### Genomic Date used:

#### Shape files downloaded online through ARCgis for North America, USA, States in the USA, Canada, Smallmouth Bass native species distributions, and rivers in teh Central Interior Highlands

# Aim 7 - Hybrid Simulation
### File: SMB_HYBRID_DETECTIVE.Rmd

Purpose: Here I am following the procedure outlined in Ebersbach et al. 2020 on assessing hybridization. I am determining the likelihood with which admixed Smallmouth Bass populations are made up of recent generation hybrids rather than being made up of ancient hybrids (or very old mixing). First, I generated a dataset of 200 diagnostic SNP loci that represent highly divergent loci between pure Neosho (HSYC population, inferred in Mixmapper) and pure Northern (WHITE and SKIA populations, inferred in Mixmapper). These diagnostic SNPs were used, because their allelic distributions within the admixed populations will clearly show whether individuals are recent or ancient-generation hybrids. I then used these same 200 diagnostic loci to create simulated recent-generation hybrid data, which I then compared to be real data using DAPC. 

### Genomic Date used:

#### Fully-filtered SMB dataset, HSYC and WHITE or HSYC and SKIA to obtain diagnostic SNPS
#### Fully-filtered SMB dataset, Admixed populations only to assess admixture

# Unused Code

All code in this file is miscellaneous, unused code from the SMB Genomics R project