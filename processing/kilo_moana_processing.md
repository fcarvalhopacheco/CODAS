# Shipboard ADCP Processing on board R/V Kilo Moana

## Introduction

Currents in the upper ocean (0-1200 m) are measured using shipboard Acoustic Doppler Current Profilers (ADCP) on board 
the R/V Kilo Moana.  Data from the ship are collected and preliminarily processed real-time using the University of Hawaii's 
CODAS processing system (https://currents.soest.hawaii.edu/home/).  This system allows for automatic quality control of 
the data and real time graphic display of current profiles and other data products while at sea.  Should any ancillary 
data stream be disrupted at sea, raw data are saved and a complete re-processing of the data is possible at a later date. 

The R/V Kilo Moana is equipped with two ADCP systems.  

```NEED TO UPDATE THE FOLLOWING:
An RD Instruments Ocean Surveyor 75 is located on the starboard side of the ship and an RD Instruments Work Horse 300 
is located on the port side both with a transducer depth of 7 m.  The Ocean Surveyor operates at 75 KHz and is able to 
profile to xxx m in broadband mode with a bin size of xx m averaging ensembles every 5 minutes.  In narrow band mode 
with xx bins, profiles can reach as deep as xxxx m. The Work Horse operates at 300 KHz profiling typically to a maximum 
of 100 m with a bin size of 4 m and averaging ensembles every 2 minutes.  Heading information is taken from the gyro 
compass and corrected using a <TSS POS/MV 320>, (an integrated inertial and GPS system).  An <Ashtech ADU5> is used as 
a heading correction device should there be a problem with the POS/MV.  Position data are provided by the POS/MV system 
with the Ashtech ADU5 and a Trimble GPS as backups.` 
```

A further step is then undertaken applying small heading corrections to the velocity data, trimming unnecessary
data from the beginning and ends of the cruise followed by a visual inspection of the final dataset.  

## Processing Overview:

- Shipboard data is taken from OTG and given to the Chief Scientist after each cruise.
Unprocessed ADCP is packaged in 4 directories (gbin, proc, raw, and rbin) in 
`/export/malino5/hot/####/underway/km###-ADCP-Data`

- Post-cruise processing of the shipboard adcp data can be done from virtual box.
[Please check VBbox installation here](https://currents.soest.hawaii.edu/docs/adcp_doc/codas_setup/virtual_computer/index.html)

## 1. Setup Directories

### 1.1 Directories for HOT processing

+ **On terminal**, connect to `HELU` and navigate to :

    ```shell script
    # $$$ = cruise number
    cd /export/malino5/hot/$$$$/underway/km$$$-ADCP-Data
    ```

    ```shell script
    # xxxx = cruise vessel number
    cd /kmxxxxxx/proc
    ```

    + In this folder, should be the different directories from each type of ADCP installed on the ship (WH300, OS75bb, OS75nb etc). Copy all folders into new folder in `/home/helu/science/HOT/data/shipadcp` 

    ```shell script
    cp –a proc /science/hot/data/shipadcp/hot<cruise_num>_proc/
    ```
    > NOTE: While the data are not processed we use the notation `_proc` on the cruise directory. 
    When finished the directory is renamed and a link from hot/adcp is made.

    + Copy previous cruises' ADCP notes

    > Navigate to a previously processed cruise  and copy over the `**_notes_wh300.txt` (and/or other ADCP’s) 
    into the unprocessed cruise directory. Open the notes with the text editor 
    (double click .txt file in the file explorer). This file contains the calibration commands entered to perform 
    ADCP calibrations, and since this .txt file will eventually be combined with another .txt file and sent to the UH
    Currents group, it’s best to use it as a guide for processing. Edit the commands in the .txt file and then copy 
    and paste them into the helu terminal; this will allow future users to track how the calibration was performed. 


### 1.2 Directories if you are using you own processing computer with VBox

+ On your local machine, create a shared folder to be used for shipboard adcp processing.

    ```shell script
    # Change <codas_processing_path> and <cruise_name> with your own folder names
    mkdir /Users/Shared/<codas_processing_path>/<cruise_name.orig>
    mkdir /Users/Shared/<codas_processing_path>/<cruise_name_proc>
    ```

+ Navigate to `<cruise_name.orig>` folder

    ```shell script
    cd /Users/Shared/<codas_processing_path>/<cruise_name.orig>
    ```

+ Copy all unprocessed cruise directories onto .orig folder

    ```shell script
    # change <user_name>, <server_address>, <remote_path_to_shipadcp_data>,  <local_path4_processing>
    rsync -avzh <user_name>@<server_address>:/<remote_path_shipadco_data>.orig <local_path4_processing> 
    ```

+ Copy all download data onto `_proc` folder

    ```shell script
    cp -r  /Users/Shared/<codas_processing_path>/<cruise_name.orig>/ /Users/Shared/<codas_processing_path>/<cruise_name_proc>/
    ```

+ Copy previous cruises' ADCP notes

    > Navigate to a previously processed cruise  and copy over the `**_notes_wh300.txt` (and/or other ADCP’s) 
    into the unprocessed cruise directory. Open the notes with the text editor 
    (double click .txt file in the file explorer). This file contains the calibration commands entered to perform 
    ADCP calibrations, and since this .txt file will eventually be combined with another .txt file and sent to the UH
    Currents group, it’s best to use it as a guide for processing. Edit the commands in the .txt file and then copy 
    and paste them into the helu terminal; this will allow future users to track how the calibration was performed. 

## 2. Remake all the plots

+ Open your virtual machine and connect to the codas_bionic_xxxxxx environment

+ On your virtual machine terminal, navigato to shared_folder that contains
    the shipboard adcp data:

    ```shell script
    # Something similar to
    cd /home/adcpproc/Desktop/<codas_processing_path>/<cruise_name>
    ```

+ In the `**notes_osxx.txt` file, edit the first command under `(0) remake all
the plots` to have the correct yearbase, then run the following command on your
VB terminal 

    ```shell script
    cd /home/adcpproc/Desktop/<codas_processing_path>/<cruise_name>/proc/os75nb

    # example for yearbase 2021
    quick_mplplots.py –-plots2run all –-yearbase 2021
    ```
    > The screen should fill with 11 plots. 


## 3. Check for gaps in heading correction

+ on your `<codas_processing_path>/<cruise_name>/proc/os75nb/`, run:

    ```shell script
    figview.py cal/rotate/*.png
    ```
    > Gaps will appear as red stars in the ADCP time-series plot shown. If there are gaps (typically there aren’t any), do the following:

    ```shell script
    cd ~/cal/rotate/

    # run the following command:
    patch_hcorr.py
    ```
    + select cleaner and box filter, this will interpolate over the gaps.

    + If you press yes after `patch_hcorr.py`, skip to section 4 after checking results, otherwise, continue:

    + Remove the earlier time-dependent heading correction:

        ```shell script
        rotate unrotate.tmp
        ```

    + Apply the new heading correction.

        ```shell script
        rotate rotate_fixed.tmp
        ```

    + Run navigation steps and inspect calibrations from original dataset

        ```shell script
        cd ../../
        quick_adcp.py –-steps2rerun navsteps:calib –-auto –-datatype uhdas
        ```

    + Check the difference between original and after patch_hcorr corrections:

        ```shell script
        dataviewer.py –c /<codas_processing_path>/<cruise_name.orig>proc/os75xx/     /<codas_processing_path>/<cruise_name_proc>/os75xx 
        ```

## 4. Check Water-track calibration 

+ Check to see that the ADCP mean and median amplitude is between [0.995 and 1.005] and phase is between [-0.5 0.5].

    ```shell script
    tail -20 cal/watertrk/adcpcal.out
    ```
    * Replace the water-track information (example below) in the `**_notes_osxxxx.txt` file. 

    ```shell script
    ** watertrack **  G
    Number of edited points:  37 out of  39
       amp   = 1.0074  + -0.0000 (t - 200.9)
       phase =   0.02  + 0.0809 (t - 200.9)
                median     mean      std
    amplitude   1.0080   1.0074   0.0054
    phase       0.0100   0.0169   0.2822
    nav - pc    0.0000   0.6757   1.7804
    var         0.0010   0.0008   0.0007
    min var     0.0000   0.0005   0.0006
    delta-u    -1.2200  -0.5232   3.7264
    delta-v     0.0700  -0.1789   3.8883 

    ```
+ Notice here how the amplitude median/mean is 1.0008/1.0074 and the phase is 0.010/0.0169 
(respectively). We will adjust amplitude and phase in later steps.

## 5. Check %good 

+ On terminal

    ```shell script
    dataviewer.py
    ```
    > If you note a low percentage of good on data:

    ```shell script
    quick_adcp.py --steps2rerun navsteps:calib --refuv_source uvship --auto
    ```

+ Check the difference from original data

    ```shell script
    dataviewer.py –c /<codas_processing_path>/<cruise_name.orig>proc/os75xx/     /<codas_processing_path>/<cruise_name_proc>/os75xx 
    ```

## 6. Remove Sections of questionable data

+ Navigate to /edit directory and run:

    ```shell script
    cd ~/proc/xxxx/edit
    gautoedit.py -n5
    ```

    > This will open up 3 windows.  One contains the time-series of U and V (partitioned into 0.8 day sections), and another 
    with options to apply edits, while the last is the latitude and longitude of the ship during the cruise. 
    Check the U and V time-series plot to look for sections of obviously bad data (bad sections should stick out against 
    the other ‘good’ velocities in the contour plot). If there are points to remove, find the ‘Manual editing’ section in 
    the editing window. Toggle which plot to remove points from (U or V) and then click on ‘Edit Now’. This will pop up 
    another window where you can highlight rectangular sections of the contour plot to remove. Once all the sections are
    highlighted, click on ‘Save Manual Edits’. When ready to move on to more data, click the arrows next to the green ‘Show’ 
    button near the top of the window to refresh the U,V time-series plot with another 0.8 days of ADCP data. When all the 
    bad data has been removed, click on the red ‘Apply Editing’ button and close out of all the windows. 


+ Apply correction 

    ```shell script
    # Make sure you are on `osxxx` folder
    quick_adcp.py --steps2rerun apply_edit:navsteps:calib  --auto
    ```

## 7. Apply new calibration

+ Now we want to correct the amplitude to the third significant figure and phase to the second significant figure. If both are positive, enter a positive correction.

    ```shell script
    #amplitude calibration:
    
    # change XXXX
    quick_adcp.py --steps2rerun rotate:navsteps:calib --rotate_amplitude XXXX --auto


    #check calibration:

    tail -20 cal/watertrk/adcpcal.out

            Number of edited points:  29 out of  33
               amp   = 0.9996  + 0.0016 (t - 238.0)
               phase =  -0.05  + -0.0170 (t - 238.0)
                        median     mean      std
    --->    amplitude   1.0000   0.9996   0.0141
            phase       0.0920  -0.0480   0.7765


    #phase calibration:
    quick_adcp.py --steps2rerun rotate:navsteps:calib --rotate_angle YYYY --auto

    #check calibration:

    tail -20 cal/watertrk/adcpcal.out

            Number of edited points:  29 out of  33
               amp   = 0.9996  + 0.0016 (t - 238.0)
               phase =   0.00  + -0.0168 (t - 238.0)
                        median     mean      std
            amplitude   1.0000   0.9996   0.0140
    --->    phase       0.1380   0.0023   0.7759
    ```


## 8. Web plots

+ Create Web plots

    ```shell script
    quick_web.py --interactive
    ```
    > This will open a few windows with the latitude and longitude time-series from the cruise. Find the ‘select’ button 
      at the bottom of the figure and use the cross to select the northbound, on-station, and southbound legs of the cruise. 
      Exit out of all the plots afterwards.

+ or you can use the same sections as were used before:
    
    ```shell script
    mkdir webpy
    cp ../<os75xx_folder>/webpy/sectinfo.txt webpy/ 
    quick_web.py --redo
    ```

## 9. Data extraction

+ Extract the data for other people to use:

    ```shell script
    #Matlab
    quick_adcp.py --steps2rerun matfiles --auto
    
    #NetCDF - change xxxxx with your adcp sensor number
    adcp_nc.py adcpdb contour/osxxxxx <cruise_folder> osxxxxx --ship_name <research_vessel_name>
    ncdump -h contour/osxxxxx.nb
    ```

## 10. Save all edits 

+ Save all edits in the `**_notes_osxxx.txt` file

    ```shell script
    cat ~/osxxxx/cruise_info.txt **_notes_osxxxx.txt > osxxxx.txt
    ```

