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
            |___sub-0000_image.nii.gz
        |___Labels[1]
            |___sub-0000_image.nii.gz
```


#Agreement measures
The agreement measures can be computed on global and local region-of-interest (ROI) level for two input images.
The

#Network metrics


#Machine learning classification



Container usage:
-----


1. Building containers

To build the validation container from inside this repository run the following command.

```bash
    docker build --rm=True \
        -t cfmvalidation:latest .
```

2. Running 

Both containers follow the same principle of modular calls to enable fast independent deployment within HPC environments.


```bash
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


