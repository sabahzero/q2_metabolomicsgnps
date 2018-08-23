# QIIME 2 Metabolomics GNPS plugin

This is a QIIME 2 plugin to analyze metabolomics data that utilizes GNPS.

## Background

Mass spectrometry detects molecules via charged surrogates called ions (i.e. mass-to-charge, m/z). MS data contains MS1 spectra (i.e. spectrum of all ions; m/z and their respective abundance) and MS2 spectra (i.e. spectrum of structural fragments of an ion). MS2. spectra are generated by imparting excess internal energy into ions which causes them to fragment. Collision induced dissociation (CID) or higher-energy collision induced dissociation (HCD) are common methods to impart energy via collisions with gas molecules. One can collect both MS1 and MS2 spectra in a single experiment using data dependent acquisition (DDA), a common approach used in untargeted metabolomics. The data contains a series of MS1 spectra from which the n most abundant m/z values are selected and fragmented serially. This cycle continues throughout the analysis of a sample. Often, liquid chromatography (or chemical separation techniques) are combined with mass spectrometry to simplify sample complexity and provide orthogonal information (e.g. retention time). Samples can be compared using either MS1 or MS2 data.

## MS2 Spectral Counts

If one were to collect data using DDA or similar method, multiple MS2 spectra from the sample ion (aka molecule) can be present in the data. One of the initial data analysis processes performed in GNPS (via MScluster) is to collapse identical spectra into a single molecular feature. The number of molecular features per sample, i.e. spectral counts, is used as a semiquantitative estimate.

## MS1 Peak Areas

Fundamentally, spectral abundance observed at at given time in a mass spectrum is related to concentration (imperfectly); however, integration of spectral abundance over time better represents the total amount of material present in the sample. Extraction ion chromatograms (XIC) are generated from all observed m/z in the MS1 of a sample, and the area under the curve (i.e. peak area) are determined via integration. Advantage of comparing samples using the MS1 is that the data can be quantitative as well as more accurately detect molecules, particularly those of low abundance, compared to MS2 (as not all ions will be selected in the top nth most abundant peaks).

## Installation

Install Qiime2 and activate environment by following the steps described [here](https://docs.qiime2.org/2018.6/install/native/).

Test if the Qiime2 installation was successful by typing the following command:

`qiime`

If Qiime2 was successfully installed, options will appear.

Then do the following command to clone the plugin scripts:

`git clone https://github.com/mwang87/q2_metabolomicsgnps`

Finally, change directory into the just newly q2_metabolomicsgnps folder and install plugin by executing the command:

`pip install -e .`

Test if the plugin was installed correctly by repeating the following command:

`qiime`

If successful, the metabolomics-gnps plugin is now listed in the options.

### Plugin Commands Listing

`qiime metabolomicsgnps` # Will list out all the commands

`qiime metabolomicsgnps gnps-clustering` # MS2 GNPS Clustering Command. This function will take as input a set of mass spectrometry files (mzXML or mzML) and a manifest file to produce a biom qza file.

`qiime metabolomicsgnps gnps-clustering-taskimport` # MS2 GNPS Clustering Command. This function will take as input an existing GNPS Molecular Networking task and a manifest file to produce a biom qza file.

`qiime metabolomicsgnps mzmine2-clustering` # MZmine2 Feature Import Command. This function will take as input a feature quantification file from MZmine2 and a manifest file and produce a biom qza file.


### Example of job GNPS-clustering job:

`qiime metabolomicsgnps gnps-clustering --p-manifest data/manifest.tsv --p-username <username> --p-password <password> --o-feature-table outputfolder`

### Example of job GNPS-clustering-taskimport job:

`qiime metabolomicsgnps gnps-clustering --p-manifest data/manifest.tsv --p-taskid cde9c128ec0c48a58e650279f1735dbc --o-feature-table outputfolder`

### Example of MZmine2-CLUSTERING job:

`qiime metabolomicsgnps mzmine2-clustering --p-manifest tests/data/mzminemanifest.csv --p-quantificationtable tests/data/mzminefeatures.csv --o-feature-table feature`

## Input Data Description/Download

### Cross-Sectional Data

### Longitudinal Data

### MZmine2 Export

MZmine2 is used to find features in the data and calculate the area under the curve. A detailed tutorial for feature finding with MZmine2 can be found [here] (https://ccms-ucsd.github.io/GNPSDocumentation/featurebasedmolecularnetworking/).

Upon finding all features according to the tutorial above, perform the following steps to export the features and their respective quantifications to be compatible with this Qiime2 plugin.

Select Export->CSV File

1. Specify .csv file name and location
2. Check “Export row ID”, “Export row m/z” and “Export row retention time”
3. Check “Peak area”

![img](img/mzmine_export.png)

4. Hit OK
5. The generated .csv file can now be used directly for further processing in Qiime2 in the Feature Based Quantification Analysis

### Manifest File Format

The manifest file specifies the location of the files that will be processed by the metabolomicsgnps plugin. It is a .CSV formatted table that contains two columns (See Figure X below). The first column indicates the ‘sample-id’ for each file, while the second column indicates its corresponding relative file path (relative to where qiime commands are called). The gnps-clustering and the mzmine2-clustering tools are using both the same manifest file.

Figure X. View of the manifest file (.CSV format). The first column indicates the ‘sample-id’ for each file, while the second column indicates its corresponding relative file path. The example file can be [downloaded here](https://github.com/mwang87/q2_metabolomicsgnps/raw/master/q2_metabolomicsgnps/tests/data/manifest.tsv).

![img](img/manifest_file.png)

## Spectrum Count Qualitative Analysis

### Food Cross Sectional Study

### Longitudinal Study

## Feature Based Quantification Analysis

### Food Cross Sectional Study

### Longitudinal Study
