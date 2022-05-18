About

This gist details how to convert illumina final report genotype data to an input useable by PLINK. From there the genotype can be quality controlled and the end results exported to a .vcf file or otherwise parsed.

# Lgen/PLINK format

lgen from the PLINK documentation "A text file with no header line, and one line per genotype call (or just not-homozygous-major calls if 'lgen-ref' was invoked) usually with the following five fields:

Family ID

Within-family ID

Variant identifier

Allele call 1 ('0' for missing)

Allele call 2"

# Map format

## Map from the PLINK documentation

"A text file with no header file, and one line per variant with the following 3-4 fields:

Chromosome code. PLINK 1.9 also permits contig names here, but most older programs do not.

Variant identifier

Position in morgans or centimorgans (optional; also safe to use dummy value of '0')

Base-pair coordinate"

# Fam format

Fam from the PLINK documentation "A text file with no header line, and one line per sample with the following six fields:

Family ID ('FID')

Within-family ID ('IID'; cannot be '0')

Within-family ID of father ('0' if father isn't in dataset)

Within-family ID of mother ('0' if mother isn't in dataset)

Sex code ('1' = male, '2' = female, '0' = unknown)

Phenotype value ('1' = control, '2' = case, '-9'/'0'/non-numeric = missing data if case/control)"

Creating the lgen and map file

The attached Rscript illumina_to_lgen creates the associated lgen and map file used by plink. It can be saved into your directory as is to run with cli. To run it from the command line you must supply it with the illumina finalreport.txt file and an outpout directory. This will output two files into the output directory "mets.map" and "mets.lgen." Not that this will overwrite any existing file in the output directory that already bears these names. Also not that for its two allele calls thsi script chooses forward allele 1 and forward allele 2.

# Example Run

* Rscript illumina_to_lgen.R --illumina PATH/TO/FinalReport.txt --outputdir PATH/TO/OutputDirectory/

This script will generate dummy values to be used by certain columns in the output file. Within the lgen file, the "fid" column is a dummy value. It is simply a duplicate of the sample ids. Within the map file, the "Genetic Distance" aka the third column is a dummy column. Its contents are all set to 0.

Creating the fam file

Creation the the fam file was done in R. Sex data was read into R as shown in the Sex_to_Fam.R code.

dummy values within the fam file there are 4 columns of dummy values. These columns are "fid", "paternal id"," maternal id", and "phenotype" corresponding to columns one, three, four, and six. As in the lgen files, fid values are duplicates of the sample ids. "Paternal id"," maternal id", and "phenotype" are all set to zero.




