# Shipboard ADCP Processing on board R/V Kilo Moana

## Introduction

Currents in the upper ocean (0-1200 m) are measured using shipboard Acoustic Doppler Current Profilers (ADCP) on board the R/V Kilo Moana.  Data from the ship are collected and preliminarily processed real-time using the University of Hawaii's CODAS processing system (http://currents.hawaii.edu).  This system allows for automatic quality control of the data and real time graphic display of current profiles and other data products while at sea.  Should any ancillary data stream be disrupted at sea, raw data are saved and a complete re-processing of the data is possible at a later date. 

The R/V Kilo Moana is equipped with two ADCP systems.  `An RD Instruments Ocean Surveyor 38 is located on the starboard side of the ship and an RD Instruments Work Horse 300 is located on the port side both with a transducer depth of 7 m.  The Ocean Surveyor operates at 38 KHz and is able to profile to 1200 m in broadband mode with a bin size of 12 m averaging ensembles every 5 minutes.  In narrow band mode with 24 m bins, profiles can reach as deep as 1500 m. The Work Horse operates at 300 KHz profiling typically to a maximum of 100 m with a bin size of 4 m and averaging ensembles every 2 minutes.  Heading information is taken from the gyro compass and corrected using a TSS POS/MV 320, (an integrated inertial and GPS system).  An Ashtech ADU5 is used as a heading correction device should there be a problem with the POS/MV.  Position data are provided by the POS/MV system with the Ashtech ADU5 and a Trimble GPS as backups.` --- UPDATED THIS TEXT with correct sensors 

A further step is then undertaken applying small heading corrections to the velocity data, trimming unnecessary data from the beginning and ends of the cruise followed by a visual inspection of the final dataset.  


## Processing Overview:

- Shipboard data is taken from OTG and given to the Chief Scientist after each cruise.
Unprocessed ADCP is packaged in 4 directories (gbin, proc, raw, and rbin) in 
`/export/malino5/hot/####/underway/km###-ADCP-Data`

- Post-cruise processing of the shipboard adcp data can be done from virtual box.
[Please check VBbox installation here](https://currents.soest.hawaii.edu/docs/adcp_doc/codas_setup/virtual_computer/index.html)

## 1. Setup Directories

> Navigate to your Shipboard ADCP data that contains the following folders:

```
/gbin
/proc
/raw
/rbin
/reports
```

### For HOT processing

+ **On terminal**, connect to `HELU` and navigate to :

```
$cd `/export/malino5/hot/####/underway/km###-ADCP-Data` 
$cd /proc
```
+ In this folder, should be the different directories from each type of ADCP installed on the ship (WH300, OS75bb, OS75nb etc). Copy all folders into new folder in `/home/helu/science/HOT/data/shipadcp` 
    
