This is the replication package for the article:
"Adopt a Monster: Engaging Developers Through Gamification"

The analyses were done with the R package running in RStudio, with the Lavaan library installed. We recommend to create a clean R project within RStudio. This package contains a number of CSV files and a script. The script assumes that all CSV files are within the same folder as the script itself.

Per agreement with the XCorp, the organization that facilitated the data collection, we cannot make the full data set available. To run the model nevertheless, this package provides the necessary matrices that can be loaded (and which are derived from the data set). The following files are provided:

 cfa-S.csv	the covariance matrix [for the CFA]
 cfa-NACOV.csv	necessary because the estimator is not standard ML, but MLM
 
 sem-S.csv	the covariance matrix [for the SEM analysis]
 sem-NACOV.csv	necessary because the estimator is not standard ML, but MLM
 h2-fscores.csv	factor scores for the 3 latent variables used for hypothesis 2


The script replication.r loads theses files and invokes the cfa() and sem() functions from the lavaan package, which is assumed to be installed. If not, run this script:

 install.packages("lavaan")


At the time of writing [July 2020], this will install version 0.6.6. This replication package is tested. If despite that there are any problems, please contact the editor who can contact us on your behalf.

