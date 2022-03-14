# Shipboard ADCP Processing on board R/V Kilo Moana

## Introduction

Currents in the upper ocean (0-1200 m) are measured using shipboard Acoustic Doppler Current Profilers (ADCP) on board the R/V Kilo Moana.  Data from the ship are collected and preliminarily processed real-time using the University of Hawaii's CODAS processing system (http://currents.hawaii.edu).  This system allows for automatic quality control of the data and real time graphic display of current profiles and other data products while at sea.  Should any ancillary data stream be disrupted at sea, raw data are saved and a complete re-processing of the data is possible at a later date. 

The R/V Kilo Moana is equipped with two ADCP systems.  An RD Instruments Ocean Surveyor 38 is located on the starboard side of the ship and an RD Instruments Work Horse 300 is located on the port side both with a transducer depth of 7 m.  The Ocean Surveyor operates at 38 KHz and is able to profile to 1200 m in broadband mode with a bin size of 12 m averaging ensembles every 5 minutes.  In narrow band mode with 24 m bins, profiles can reach as deep as 1500 m. The Work Horse operates at 300 KHz profiling typically to a maximum of 100 m with a bin size of 4 m and averaging ensembles every 2 minutes.  Heading information is taken from the gyro compass and corrected using a TSS POS/MV 320, (an integrated inertial and GPS system).  An Ashtech ADU5 is used as a heading correction device should there be a problem with the POS/MV.  Position data are provided by the POS/MV system with the Ashtech ADU5 and a Trimble GPS as backups.

A further step is then undertaken applying small heading corrections to the velocity data, trimming unnecessary data from the beginning and ends of the cruise followed by a visual inspection of the final dataset.  


## Step 1:
- [Download Python 3.6 Anaconda](https://www.anaconda.com/download/#linux).We will set up a python 3.6 environment this time

  + Follow instructions [here]( https://docs.anaconda.com/anaconda/install/linux)
  + If you have already installed the anaconda, **GO to STEP 2**, otherwise keep following the instructions:

- Check if anaconda installer have put a ``anaconda/bin`` subdirectory in your ``$PATH`` environment
    
  + **On terminal**, navigate to :
  
    ```
    $cd /home/your_username 
    ```
 
  + **Open** your bash_profile:

    ```
    $vi .bash_profile
    ```

  + You should be able to see something like:

    ```
    # added by Anaconda3 installer
    export PATH="/home/your_username/anaconda3/bin:$PATH"
    ```

  + **Close** editor:

    ```
    :wq
    ```

  * **OBS:** If you can't find the ``$PATH`` on ``bash_profile``, check at the end of the ``.bashrc`` file:

    + **Open** bashrc:

      ```
      $vi .bashrc
      ```

    + **Move cursor** to the end of file:
      ```
      Shift + g
      ```

