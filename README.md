# Nomads Classifier
Nomads Classier is a python package for classifying GABA and Non-GABA synapses in array tomography (AT) dataset of the brain.

## Contents
- [Overview](#overview)
- [System Requirements](#system-requirements)
- [Usage Guide](#usage-guide)
- [Limiations](#limitations)

## Overview
Array Tomography (AT) has made it possible to image huge regions of synapses in the brain. One area of interest is to identify and classify different types of synapses. **Nomads Classifier** is a package for classifying synapses that are identified in the AT dataset. The classifier was pretrained on professionally annotated dataset, which included about 1000 synapses (~900 non-GABA, ~100 GABA). 

## System Requirements
  - **Nomads Classifier** was developed in Python 3.6. Currently, there is no plan to support Python 2.
  - Was developed and tested primarily on Mac OS (Sierra 10.12.6). It does not currently support Windows.
  - Requires no non-standard hardware to run.
  - Part of the Nomads Deploy pipeline.

The following lists the dependencies for **Nomads Classifier**. As you can see, it is very robust.  

```
numpy>=1.13.1
```

## Usage Guide
### Github

    git clone https://github.com/neurodata-nomads/nomads_classifier
    cd nomads_classifier

### Running on data
It is highly recommended that you run **Nomads Classifier** as part of the [**Nomads Cloud Service**](https://github.com/neurodata-nomads/nomads_cloud). However, if you do wish to run the classifier alone, **Nomads Classifier** expects the data in the following format.
```
raw_data : dictionary 
    Each key is channel name and value is a 3d-numpy array.
centroids : list 
    List of of tuples in (z, y, x) format of the centroids of each 
    annotation/prediction.
```
Once the data is obtained, you can run
```
from nomads_classifier import classifier_pipeline

labels = classifier_pipeline(raw_data=raw_data, centroids=centroids)
```

The classifier will output a numpy array with the size of list of centroids. The three labels will be:
```
0 : not classified due to its proximity to the edges
1 : non-GABA synapse
2 : GABA synapse
```

## Limitations
The classifier was trained using the following eight AT channels:
```
GABA
GAD
Gephyrin
GluN
PSD
Synapsin
TdTomato
VGlut
```
As such, the classifier will expect channels that are the same, if not similar, to the channels used in training of the classifier. Otherwise, results may be non-sensical.