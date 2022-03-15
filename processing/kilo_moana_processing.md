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

### 1.1 Directories for HOT processing

+ **On terminal**, connect to `HELU` and navigate to :

```sh
$cd /export/malino5/hot/$$$$/underway/km$$$-ADCP-Data  $$$ = cruise_number
```

```sh
cd /proc
```

+ In this folder, should be the different directories from each type of ADCP installed on the ship (WH300, OS75bb, OS75nb etc). Copy all folders into new folder in `/home/helu/science/HOT/data/shipadcp` 

```sh
$cp –a proc /science/hot/data/shipadcp/hot<cruise_num>_proc/
```
> NOTE: While the data are not processed we use the notation `_proc` on the cruise directory. 
When finished the directory is renamed and a link from hot/adcp is made.

+ Copy previous cruises' ADCP notes

> Navigate to a previously processed cruise  and copy over the `**_notes_wh300.txt` (and/or other ADCP’s) into the unprocessed cruise directory. Open the notes with the text editor (double click .txt file in the file explorer). This file contains the calibration commands entered to perform ADCP calibrations, and since this .txt file will eventually be combined with another .txt file and sent to the UH Currents group, it’s best to use it as a guide for processing. Edit the commands in the .txt file and then copy and paste them into the helu terminal; this will allow future users to track how the calibration was performed. 


### 1.2 Directories if you are using you own processing computer with VBox

+ On your local machine, create a shared folder to be used
for shipboard adcp processing.

```
# Change <codas_processing_path> and <cruise_name> with your own folder names
$mkdir /Users/Shared/<codas_processing_path>/<cruise_name.orig>
$mkdir /Users/Shared/<codas_processing_path>/<cruise_name_proc>
```
+ Navigate to `<cruise_name.orig>` folder

```
$cd /Users/Shared/<codas_processing_path>/<cruise_name.orig>
```

+ Copy all unprocessed cruise directories onto .orig folder

```
# change <user_name>, <server_address>, <remote_path_to_shipadcp_data>,  <local_path4_processing>
$rsync -avzh <user_name>@<server_address>:/<remote_path_shipadco_data>.orig <local_path4_processing> 
```

+ Copy all download data onto `_proc` folder

```
$cp -r  /Users/Shared/<codas_processing_path>/<cruise_name.orig>/ /Users/Shared/<codas_processing_path>/<cruise_name_proc>/
```

+ Copy previous cruises' ADCP notes

> Navigate to a previously processed cruise  and copy over the `**_notes_wh300.txt` (and/or other ADCP’s) into the unprocessed cruise directory. Open the notes with the text editor (double click .txt file in the file explorer). This file contains the calibration commands entered to perform ADCP calibrations, and since this .txt file will eventually be combined with another .txt file and sent to the UH Currents group, it’s best to use it as a guide for processing. Edit the commands in the .txt file and then copy and paste them into the helu terminal; this will allow future users to track how the calibration was performed. 


## 2. Remake all the plots

+ Open your virtual machine and connect to the codas_bionic_xxxxxx environment

+ On your virtual machine terminal, navigato to shared_folder that contains
the shipboard adcp data:

```sh
# Something similar to
$cd /home/adcpproc/Desktop/<codas_processing_path>/<cruise_name>
```

+ In the `**notes_osxx.txt` file, edit the first command under `(0) remake all
the plots` to have the correct yearbase, then run the following command on your
VB terminal 

```sh
$cd /home/adcpproc/Desktop/<codas_processing_path>/<cruise_name>/proc/os75nb

# example for yearbase 2021
$quick_mplplots.py –-plots2run all –-yearbase 2021
```
> The screen should fill with 11 plots. 


## 3. Check for haps in heading correction

+ on your `<codas_processing_path>/<cruise_name>/proc/os75nb/`, run:

```
$figview.py cal/rotate/*.png
```
> Gaps will appear as red stars in the ADCP time-series plot shown. If there are gaps (typically there aren’t any), do the following:

    ```sh
    $cd ~/cal/rotate/
    
    # run the following command:
    patch_hcorr.py
    
    ```
> select cleaner and box filter, this will interpolate over the gaps.

+ Remove the earlier time-dependent heading correction:

```sh
$rotate unrotate.tmp
```

+ Apply the new heading correction.

```sh
$rotate rotate_fixed.tmp
```

+ Run navigation steps and inspect calibrations from original dataset

```sh
$cd ../../
$ quick_adcp.py –-steps2rerun navsteps:calib –-auto –-datatype uhdas
```

+ Check the difference between original and after patch_hcorr corrections:

```sh
$dataviewer.py –c /<codas_processing_path>/<cruise_name.orig>proc/os75xx/     /<codas_processing_path>/<cruise_name_proc>/os75xx 
```
> xx = os75nb or os75bb folders.

## 4. Check Water-track calibration 

