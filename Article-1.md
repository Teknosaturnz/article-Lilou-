R Notebook
================

article ch ngement du pattern sed -i ‘s\|<ftp://%7Chttps://%7Cg>;
s\|wget -nc\|wget -c –tries=10 –retry-connrefused –waitretry=5\|g’
download.sh

pour extraire les données source
../ena-file-download-read_run-PRJEB40676-fastq_ftp-20261009-0817.sh

../ = niveau au dessus

pour classer les données entre reverse et forwar attention être dans
l’evt global mv data_2/\*\_1.fastq.gz sample data_2 mv
data_2/\*\_2.fastq.gz sample

installer les métadonnées

``` r
if (!requireNamespace("BiocManager", quietly = TRUE))
    install.packages("BiocManager")
BiocManager::install("dada2", version = "3.23")
```

    ## 'getOption("repos")' replaces Bioconductor standard repositories, see
    ## 'help("repositories", package = "BiocManager")' for details.
    ## Replacement repositories:
    ##     CRAN: https://p3m.dev/cran/__linux__/resolute/latest

    ## Bioconductor version 3.23 (BiocManager 1.30.27), R 4.6.2 (2026-10-29)

    ## Warning: package(s) not installed when version(s) same as or greater than current; use
    ##   `force = TRUE` to re-install: 'dada2'

    ## Installation paths not writeable, unable to update packages
    ##   path: /usr/local/lib/R/library
    ##   packages:
    ##     mgcv, nlme
