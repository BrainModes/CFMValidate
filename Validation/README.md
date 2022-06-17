*CFMValidate*: an extensive validation of the general impact of cost function masking in MRI
=========================================================



This code has been created by the [brainsimulation group](http://brainsimulation.org/) as part of an ongoing effort
for the validation of processing methodologies in the presence of pathological abnormalities such as ischemic stroke lesions.
The project is split in a processing part *CFMValidate - Processing* and a validation part *CFMValidate - Validation* both seperately integrated within docker container images to enable reproducability and ease of use on HPC environments.



About
-----

*CFMValidate - Validation* computes a range of agreement measures and analysis to validate the CFM impact along 
the various steps of the processing pipeline. Following [Bertels et al. (2019)](https://link.springer.com/chapter/10.1007/978-3-030-32245-8_11) we integrated *dice score*, *jaccard score*
and *volume difference* as agreement measures for comparison of processing results of the *CFMValidate - Processing*
framework. We also integrate machine learning based classification of binary groups (e.g. ADNI healthy controls
versus MCI/AD patients) following ([Baghwat et al. (2021)](https://pubmed.ncbi.nlm.nih.gov/33481004/)) to approximate CFM impact on classical group level analysis.


Steps
-----

The validation framework contains groups of measures to compute based on the input and level of processing.
The expected input follows the same schema for all validation steps:

```bash
# input folder structure
/Path/To/Data/
        |___Labels[0]
            |___sub-0000_processed.nii.gz
        |___Labels[1]
            |___sub-0000_processed.nii.gz
```


## Agreement measures
Agreement measures can be computed on global and local region-of-interest (ROI) level for two input images.
The given output is one or several .txt files containing the corresponding measures for all subjects present in both directories (Labels[0] and Labels[1]). 
If the provided input images do not overlap between labels directories an error message is thrown including the mismatching image files.
Output examples for both levels of agreement measures

# global agreement measures (e.g. cortical ribbon agreement)
creating a single .txt file containing three global agreement measures for all subjects
```bash
# dice, measure_jaccard, measure_difference
9.136842269986961140e-01,8.410852992351787183e-01,-9.919975197253724786e-03
9.237416153851532030e-01,8.582898201677900962e-01,-6.901903392779281353e-03

```
# global network agreement measures
Global agreement can also be computed as the distance between created connectomes. Currently implemented are
*pearson correlation* between vectorized connectomes and *hausdorff distance*.


# local agreement measures (e.g. ROI dice agreement)
creating various .txt files containing agreement arrays with N(subjectlist) x N(ROIs)

```bash
# ['0', '1', '2', ...
8.934348239771645606e-01,9.763513513513513153e-01,8.810365135453475105e-01,...
8.527709913848527945e-01,9.609929078014184389e-01,9.088541666666666297e-01,...

```

## Network metrics
For created functional connectomes (FC) a range of network theoretic measures are computed to evaluate 
connectomics based differences caused by CFM.

Currently implemented are basic metrics *node degree*, *clustering coefficient*, *betweenness centrality*.
Again metrics can be computed on local and global scale following the same output structure as above agreement measures.

## Machine learning classification
To investigate CFM impact on a general downstream analysis step *CFMValidate - validation* also performs
*random forest* based classification based on provided input matrices. The default parameters perform a 10% cross validation step
with rebalanced classes and 100 iteration to create a robust estimate of performance.
Created output contains a .txt file with accuracy measures for each iteration and a .txt file with the corresponding
feature importance (Gini importance criteria) for each iteration.


Container usage:
-----


1. Building containers

To build the validation container from inside this repository run the following command.

```bash
    docker build --rm=True \
        -t cfmvalidation:latest .
```

2. Running 

Example run commands with required input parameters for all validation steps described above.

```bash
# computing global agreement measures on extracted cortical ribbon after registration with FSL
    docker run \
        --v "/Path/to/data/":"/data" \
        --e Input="/data/sub-0001_T1w.nii.gz" \
        --e Steps="reg" \
        --e Algorithm="FSL" \
        --e Mask="True" \
        cfmprocessing:latest
```

# Required arguments
```bash
    Steps = <string defining processing / validation step to perform>

```

Acknowledgements
----------------

Please acknowledge this work using the citation provided via the [figshare](https://figshare.com/account/projects/141644/articles/20079056) item for this project.


Maintenance
----------------

This code was originally developed for the above mentioned project. Maintenance will be performed non regularly.
Maintainer: patrik.bey@bih-charite.de


