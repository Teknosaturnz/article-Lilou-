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

``` r
library(dada2); packageVersion("dada2")
```

    ## Loading required package: Rcpp

    ## [1] '1.40.0'

``` r
path <- "~/article-Lilou-/sample" # CHANGE ME to the directory containing the fastq files after unzipping.
list.files(path)
```

    ##   [1] "ERR4658163_1.fastq.gz" "ERR4658163_2.fastq.gz" "ERR4658164_1.fastq.gz"
    ##   [4] "ERR4658164_2.fastq.gz" "ERR4658165_1.fastq.gz" "ERR4658165_2.fastq.gz"
    ##   [7] "ERR4658166_1.fastq.gz" "ERR4658167_1.fastq.gz" "ERR4658167_2.fastq.gz"
    ##  [10] "ERR4658168_1.fastq.gz" "ERR4658168_2.fastq.gz" "ERR4658169_1.fastq.gz"
    ##  [13] "ERR4658169_2.fastq.gz" "ERR4658170_1.fastq.gz" "ERR4658170_2.fastq.gz"
    ##  [16] "ERR4658171_1.fastq.gz" "ERR4658171_2.fastq.gz" "ERR4658172_1.fastq.gz"
    ##  [19] "ERR4658172_2.fastq.gz" "ERR4658173_1.fastq.gz" "ERR4658173_2.fastq.gz"
    ##  [22] "ERR4658174_1.fastq.gz" "ERR4658174_2.fastq.gz" "ERR4658175_1.fastq.gz"
    ##  [25] "ERR4658175_2.fastq.gz" "ERR4658176_1.fastq.gz" "ERR4658176_2.fastq.gz"
    ##  [28] "ERR4658177_1.fastq.gz" "ERR4658177_2.fastq.gz" "ERR4658178_1.fastq.gz"
    ##  [31] "ERR4658178_2.fastq.gz" "ERR4658179_1.fastq.gz" "ERR4658179_2.fastq.gz"
    ##  [34] "ERR4658180_1.fastq.gz" "ERR4658180_2.fastq.gz" "ERR4658181_1.fastq.gz"
    ##  [37] "ERR4658181_2.fastq.gz" "ERR4658182_1.fastq.gz" "ERR4658182_2.fastq.gz"
    ##  [40] "ERR4658183_1.fastq.gz" "ERR4658183_2.fastq.gz" "ERR4658184_1.fastq.gz"
    ##  [43] "ERR4658184_2.fastq.gz" "ERR4658185_1.fastq.gz" "ERR4658185_2.fastq.gz"
    ##  [46] "ERR4658186_1.fastq.gz" "ERR4658186_2.fastq.gz" "ERR4658187_1.fastq.gz"
    ##  [49] "ERR4658187_2.fastq.gz" "ERR4658188_1.fastq.gz" "ERR4658188_2.fastq.gz"
    ##  [52] "ERR4658189_1.fastq.gz" "ERR4658189_2.fastq.gz" "ERR4658190_1.fastq.gz"
    ##  [55] "ERR4658190_2.fastq.gz" "ERR4658191_1.fastq.gz" "ERR4658191_2.fastq.gz"
    ##  [58] "ERR4658192_1.fastq.gz" "ERR4658192_2.fastq.gz" "ERR4658193_1.fastq.gz"
    ##  [61] "ERR4658193_2.fastq.gz" "ERR4658194_1.fastq.gz" "ERR4658194_2.fastq.gz"
    ##  [64] "ERR4658196_1.fastq.gz" "ERR4658196_2.fastq.gz" "ERR4658197_1.fastq.gz"
    ##  [67] "ERR4658197_2.fastq.gz" "ERR4658198_1.fastq.gz" "ERR4658198_2.fastq.gz"
    ##  [70] "ERR4658199_1.fastq.gz" "ERR4658199_2.fastq.gz" "ERR4658200_1.fastq.gz"
    ##  [73] "ERR4658200_2.fastq.gz" "ERR4658201_1.fastq.gz" "ERR4658201_2.fastq.gz"
    ##  [76] "ERR4658202_1.fastq.gz" "ERR4658202_2.fastq.gz" "ERR4658203_1.fastq.gz"
    ##  [79] "ERR4658203_2.fastq.gz" "ERR4658204_1.fastq.gz" "ERR4658204_2.fastq.gz"
    ##  [82] "ERR4658205_1.fastq.gz" "ERR4658205_2.fastq.gz" "ERR4658206_1.fastq.gz"
    ##  [85] "ERR4658206_2.fastq.gz" "ERR4658207_1.fastq.gz" "ERR4658207_2.fastq.gz"
    ##  [88] "ERR4658208_1.fastq.gz" "ERR4658208_2.fastq.gz" "ERR4658209_1.fastq.gz"
    ##  [91] "ERR4658209_2.fastq.gz" "ERR4658210_1.fastq.gz" "ERR4658210_2.fastq.gz"
    ##  [94] "ERR4658211_1.fastq.gz" "ERR4658211_2.fastq.gz" "ERR4658212_1.fastq.gz"
    ##  [97] "ERR4658212_2.fastq.gz" "ERR4658214_1.fastq.gz" "ERR4658214_2.fastq.gz"
    ## [100] "ERR4658215_1.fastq.gz" "ERR4658215_2.fastq.gz" "ERR4658216_1.fastq.gz"
    ## [103] "ERR4658216_2.fastq.gz" "ERR4658217_1.fastq.gz" "ERR4658217_2.fastq.gz"
    ## [106] "ERR4658218_1.fastq.gz" "ERR4658218_2.fastq.gz" "ERR4658219_1.fastq.gz"
    ## [109] "ERR4658219_2.fastq.gz" "ERR4658220_1.fastq.gz" "ERR4658220_2.fastq.gz"
    ## [112] "ERR4658221_1.fastq.gz" "ERR4658221_2.fastq.gz" "ERR4658222_1.fastq.gz"
    ## [115] "ERR4658222_2.fastq.gz" "ERR4658223_1.fastq.gz" "ERR4658223_2.fastq.gz"
    ## [118] "ERR4658224_1.fastq.gz" "ERR4658224_2.fastq.gz" "ERR4658225_1.fastq.gz"
    ## [121] "ERR4658225_2.fastq.gz" "ERR4658226_1.fastq.gz" "ERR4658226_2.fastq.gz"
    ## [124] "ERR4658227_1.fastq.gz" "ERR4658227_2.fastq.gz" "ERR4658228_1.fastq.gz"
    ## [127] "ERR4658228_2.fastq.gz" "ERR4658229_1.fastq.gz" "ERR4658229_2.fastq.gz"
    ## [130] "ERR4658230_1.fastq.gz" "ERR4658230_2.fastq.gz" "ERR4658231_1.fastq.gz"
    ## [133] "ERR4658231_2.fastq.gz" "ERR4658232_1.fastq.gz" "ERR4658232_2.fastq.gz"
    ## [136] "ERR4658233_1.fastq.gz" "ERR4658233_2.fastq.gz" "ERR4658234_1.fastq.gz"
    ## [139] "ERR4658234_2.fastq.gz" "ERR4658235_1.fastq.gz" "ERR4658235_2.fastq.gz"
    ## [142] "ERR4658236_1.fastq.gz" "ERR4658236_2.fastq.gz" "ERR4658237_1.fastq.gz"
    ## [145] "ERR4658237_2.fastq.gz" "ERR4658238_1.fastq.gz" "ERR4658238_2.fastq.gz"
    ## [148] "ERR4658239_1.fastq.gz" "ERR4658239_2.fastq.gz" "ERR4658240_1.fastq.gz"
    ## [151] "ERR4658240_2.fastq.gz" "ERR4658241_1.fastq.gz" "ERR4658241_2.fastq.gz"
    ## [154] "ERR4658242_1.fastq.gz" "ERR4658242_2.fastq.gz" "ERR4658243_1.fastq.gz"
    ## [157] "ERR4658243_2.fastq.gz" "ERR4658244_1.fastq.gz" "ERR4658244_2.fastq.gz"
    ## [160] "ERR4658245_1.fastq.gz" "ERR4658245_2.fastq.gz" "ERR4658246_1.fastq.gz"
    ## [163] "ERR4658246_2.fastq.gz" "ERR4658247_1.fastq.gz" "ERR4658247_2.fastq.gz"
    ## [166] "ERR4658248_1.fastq.gz" "ERR4658248_2.fastq.gz" "ERR4658249_1.fastq.gz"
    ## [169] "ERR4658249_2.fastq.gz" "ERR4658250_1.fastq.gz" "ERR4658250_2.fastq.gz"
    ## [172] "ERR4658251_1.fastq.gz" "ERR4658251_2.fastq.gz" "ERR4658252_1.fastq.gz"
    ## [175] "ERR4658252_2.fastq.gz" "ERR4658253_1.fastq.gz" "ERR4658253_2.fastq.gz"
    ## [178] "ERR4658254_1.fastq.gz" "ERR4658254_2.fastq.gz" "ERR4658255_1.fastq.gz"
    ## [181] "ERR4658255_2.fastq.gz" "ERR4658256_1.fastq.gz" "ERR4658256_2.fastq.gz"
    ## [184] "ERR4658257_1.fastq.gz" "ERR4658257_2.fastq.gz" "ERR4658258_1.fastq.gz"
    ## [187] "ERR4658258_2.fastq.gz" "ERR4658259_1.fastq.gz" "ERR4658259_2.fastq.gz"
    ## [190] "ERR4658260_1.fastq.gz" "ERR4658260_2.fastq.gz" "ERR4658261_1.fastq.gz"
    ## [193] "ERR4658261_2.fastq.gz" "ERR4658262_1.fastq.gz" "ERR4658262_2.fastq.gz"
    ## [196] "ERR4658263_1.fastq.gz" "ERR4658263_2.fastq.gz" "ERR4658264_1.fastq.gz"
    ## [199] "ERR4658264_2.fastq.gz" "ERR4658265_1.fastq.gz" "ERR4658265_2.fastq.gz"
    ## [202] "ERR4658266_1.fastq.gz" "ERR4658266_2.fastq.gz" "ERR4658267_1.fastq.gz"
    ## [205] "ERR4658267_2.fastq.gz" "ERR4658268_1.fastq.gz" "ERR4658268_2.fastq.gz"
    ## [208] "ERR4658269_1.fastq.gz" "ERR4658269_2.fastq.gz" "ERR4658270_1.fastq.gz"
    ## [211] "ERR4658270_2.fastq.gz" "ERR4658271_1.fastq.gz" "ERR4658271_2.fastq.gz"
    ## [214] "ERR4658272_1.fastq.gz" "ERR4658272_2.fastq.gz" "ERR4658273_1.fastq.gz"
    ## [217] "ERR4658273_2.fastq.gz" "ERR4658274_1.fastq.gz" "ERR4658274_2.fastq.gz"
    ## [220] "ERR4658275_1.fastq.gz" "ERR4658275_2.fastq.gz" "ERR4658276_1.fastq.gz"
    ## [223] "ERR4658276_2.fastq.gz" "ERR4658277_1.fastq.gz" "ERR4658277_2.fastq.gz"
    ## [226] "ERR4658278_1.fastq.gz" "ERR4658278_2.fastq.gz" "ERR4658279_1.fastq.gz"
    ## [229] "ERR4658279_2.fastq.gz" "ERR4658280_2.fastq.gz" "ERR4658281_1.fastq.gz"
    ## [232] "ERR4658281_2.fastq.gz" "ERR4658282_1.fastq.gz" "ERR4658282_2.fastq.gz"
    ## [235] "ERR4658283_1.fastq.gz" "ERR4658283_2.fastq.gz" "ERR4658284_1.fastq.gz"
    ## [238] "ERR4658284_2.fastq.gz" "ERR4658285_1.fastq.gz" "ERR4658285_2.fastq.gz"
    ## [241] "ERR4658286_1.fastq.gz" "ERR4658286_2.fastq.gz" "ERR4658287_1.fastq.gz"
    ## [244] "ERR4658287_2.fastq.gz" "ERR4658288_1.fastq.gz" "ERR4658288_2.fastq.gz"
    ## [247] "ERR4658289_1.fastq.gz" "ERR4658289_2.fastq.gz" "ERR4658290_1.fastq.gz"
    ## [250] "ERR4658290_2.fastq.gz"

``` r
fnFs <- sort(list.files(path, pattern="_1.fastq.gz", full.names = TRUE))
fnRs <- sort(list.files(path, pattern="_2.fastq.gz", full.names = TRUE))
# Extract sample names, assuming filenames have format: SAMPLENAME_XXX.fastq
sample.names <- sapply(strsplit(basename(fnFs), "_"), `[`, 1)
```
