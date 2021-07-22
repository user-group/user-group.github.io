## Replication script for "Adopt a Monster".
## July 2020

library(lavaan)

## CFA model: Note that the construct "expertise" is included here as the CFA is for validating 
## the measurement model. However, it is NOT included in the SEM model (below) because expertise
## is used only in evaluating Hypothesis 2, which is done in a separate model (because Lavaan, nor
## most other packages such as the popular Amos do not support moderation with latent variables).
model.cfa <- '
  deslearn      =~ lrn_lang + lrn_skills
  expertise     =~ has_lang + has_skills
  gamepart      =~ meetups  + chat_handle + ln_chsolv
  ident         =~ ID1 + ID2 
  humcap        =~ HC1 + HC2 + HC3
  jobsat        =~ JS1 + JS2 + JS4
  knowshar      =~ KS1 + KS2 + KS3
  deveng        =~ ident  + humcap
'

## Load NACOV and covariance matrix and run the CFA.
CFA.NACOV <- as.matrix(read.csv("cfa-nacov.csv", header=F))
CFA.S     <- as.matrix(read.csv("cfa-s.csv", header=T, row.names=1))
cfa.fit   <- cfa(model=model.cfa, NACOV=CFA.NACOV, sample.cov=CFA.S, sample.nobs=155, estimator="MLM")

## Print fit measures of interest. The "robust" versions are used that are generated with the MLM estimator. 
## The "normal" values for these measures are also printed in the output (with summary(cfa.fit, fit.measures=T), 
## but those should be ignored)
##
fitMeasures(cfa.fit, c("cfi.robust", "rmsea.robust", "srmr", "tli.robust", 
                       "chisq.scaled", "df", "rmsea.ci.upper.robust", "rmsea.ci.lower.robust"))

## Chi-sq/df ratio
cfa.df    <- fitMeasures(cfa.fit, c("df"))[[1]]
cfa.chisq <- fitMeasures(cfa.fit, c("chisq.scaled"))[[1]]
cfa.ratio <- cfa.chisq / cfa.df
cfa.ratio



## Define the model
model.sem <- '
  # latent variables
  deslearn =  ~ lrn_lang  + lrn_skills
  gamepart =  ~ ln_chsolv + meetups + chat_handle
  ident    =  ~ ID1 + ID2
  humcap   =  ~ HC1 + HC2 + HC3
  jobsat   =  ~ JS1 + JS2 + JS4
  knowshar =  ~ KS1 + KS2 + KS3
  deveng   =  ~ ident + humcap

  # regressions; use labels for calculation of indirect effects
  gamepart ~ deslearn   + ln_age   + isman  + ln_tenure
  deveng   ~ p*gamepart + ln_age   + isman  + ln_tenure
  jobsat   ~ j*deveng   + gamepart + ln_age + isman + ln_tenure
  knowshar ~ k*deveng   + gamepart + ln_age + isman + ln_tenure

  # covariances of control variables
  deslearn  ~~ ln_age
  deslearn  ~~ isman
  deslearn  ~~ ln_tenure
  ln_age    ~~ isman
  ln_age    ~~ ln_tenure
  ln_tenure ~~ isman

  # Ensure KS and JS are not correlated.
  knowshar ~~ 0*jobsat

  # Calculate indirect paths from participation to js and ks
  ind_js := p*j
  ind_ks := p*k
  ind_total := (p*k) + (p*j)
'


## Load NACOV and covariance matrix
NACOV   <- as.matrix(read.csv("sem-nacov.csv", header=F))
S       <- as.matrix(read.csv("sem-s.csv", header=T, row.names=1))

## Fit model with MLM estimator
sem.fit <- sem(model=model.sem, NACOV=NACOV, sample.cov=S, sample.nobs=155, estimator="MLM")

## Print results
summary(sem.fit, fit.measures=T,standardized=T, ci=T)

## Print fit measures of interest
fitMeasures(sem.fit, c("cfi.robust", "rmsea.robust", "srmr", "tli.robust", 
                       "chisq.scaled", "df", "rmsea.ci.upper.robust", "rmsea.ci.lower.robust"))

## Chi-sq/df ratio
cov.df    <- fitMeasures(sem.fit, c("df"))[[1]]
cov.chisq <- fitMeasures(sem.fit, c("chisq.scaled"))[[1]]
cov.ratio <- cov.chisq / cov.df
cov.ratio



## Hypothesis 2 is a moderating hypothesis.
# We rely on factor scores [single values to represent latent variables, 
# making them "observed variables" in essence, but also ignoring the error term.]

# Read the factor scores from the file provided.
h2.fscores <-as.matrix(read.csv("h2-fscores.csv", header=T, row.names=1))

# Define the interaction model for Hypothesis 2
model.h2.sem <- 'gamepart ~ deslearn + expertise + expertise:deslearn'

# Run the model
fit.h2       <- sem(model.h2.sem, estimator="MLM", data=h2.fscores)

# Inspect results
summary(fit.h2, fit.measures=T,standardized=T, ci=T)
