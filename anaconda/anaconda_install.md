# Intalling CODAS software on Anaconda
- I m using Ubuntu 16.04
- This is a quick help ! visit [here](https://currents.soest.hawaii.edu/docs/adcp_doc/codas_setup/anaconda_install/index.html) for full explanation

## Step 1:
1.1 Download Python 3.6 [Anaconda](https://www.anaconda.com/download/#linux).We will set up a python 2.7 environment

- Follow instructions [here]( https://docs.anaconda.com/anaconda/install/linux)

- Check if anaconda installer have put a ``anaconda/bin`` subdirectory in your ``$PATH`` environment


### On Terminal
Navigate to:

    $cd /home/your_username  

Open your bash_profile:

    $vi .bash_profile

You should be able to see something like:

    # added by Anaconda3 installer
    export PATH="/home/your_username/anaconda3/bin:$PATH"

Close editor:
    :wq

If you can't find the ``$PATH`` on ``bash_profile``, check at the end of the ``.bashrc`` file:

Open bashrc
   
    $vi .bashrc

Move cursor to the end of file 
    
    Shift + g

## Step 2:
- Create a Python 2.7 environment in your anaconda

### On Terminal
Create py27 environment:
    $conda create -n py27 python=2.7 anaconda
    
    # Activate Python 2.7
    $source activate py27

## Step 3:
- Install Anaconda packages 

### On Terminal:

Install 4 packages:
    
    $conda install basemap          # If conflict:  conda install anaconda=custom basemap 
    $conda install netcdf4          # For Ubuntu 14.04:  conda install -c conda-forge netcdf4   
    $conda install wxpython=3       # For most recent version: conda install wxpython
    $conda install future

## Step 4:
- Install Mercurial package 

### On Terminal

    $sudo apt-get install mercurial
   > The link in the [website]( https://currents.soest.hawaii.edu/docs/adcp_doc/codas_setup/anaconda_install/index.html) seems to be broken

## Step 5:
- Create alias

### On Terminal
   
Navigate to:
    $cd /home/your_username
 
Open bash_profile:
    $vi .bash_profile

Add the following aliases to your .bash_profile file:
    alias dv="dataviewer.py "
    alias fv="figview.py"
    alias gg="gautoedit.py -n6"

Close editor:
    :wq
 
Update your .bash_profile:
    $source .bash_profile

Activate python 2.7 again:
    $source activate py27 

## Step 6:
- Get CODAS Mercurial Components

### On Terminal

Create directories and give permission to users:
    $sudo mkdir /home/adcpcode
    $sudo chown youruser:yourgroup /home/adcpcode

Create subdirectories:
    $mkdir /home/adcpcode/programs  # This is for mercurial repositories
    $mkdir /home/adcpcode/topog     # For topography plots 

Navigate to programs:
    $cd /home/adcpcode/programs

Clone these (4) repositories as follows:
    $hg clone   http://currents.soest.hawaii.edu/hg/codas3          codas3
    $hg clone   http://currents.soest.hawaii.edu/hg/pycurrents      pycurrents
    $hg clone   http://currents.soest.hawaii.edu/hg/onship          onship
    $hg clone   http://currents.soest.hawaii.edu/hg/uhdas           uhdas

## Step 7:
- Compile CODAS and Python extension

### On Terminal
   
Navite to codas3:
    $cd /home/adcpcode/programs/codas3

Compile and install codas3:
    $./waf configure --python_env
    $./waf build
    $./waf install
    $cd ..

For pycurrents:
    $cd pycurrents
    $./runsetup.py
    $cd ..
    
Install uhdas and onship:
    $cd uhdas 
    $./runsetup.py    #  This seems to be wrong on website runsetup.py insted of ./runsetup.py
    $ cd..
 
Install onship:
    $cd onship
    $python setup.py build
    $python setup.py install
    $cd..

## Step 8 

- Install Topography

- Download <ftp://currents.soest.hawaii.edu/pub/outgoing/etopo1_for_pycurrents.zip> 

[here](ftp://currents.soest.hawaii.edu/pub/outgoing/etopo1_for_pycurrents.zip)

<ftp://currents.soest.hawaii.edu/pub/outgoing/etopo1_for_pycurrents.zip>


- Unzip it into /home/adcpcode/topog/etopo

### On Terminal
   
Install unzip on ubuntu if you don't have it:
    $sudo apt-get install unzip
    
Navigate to your Download folder:
    $cd ~/Downloads

Unzip the file:
    unzip etopo1_for_pycurrents.zip -d /home/adcpcode/topog/etopo
   
    # If it's already unziped, then:
    mv etopo1_for_pycurrents/* /home/adcpcode/topog/etopo

You should be able to see something like:
    etopo1_ice_g_i2.bin
    etopo1_ice_g_i2.hdr
    etopo1_ice_g_i2_s3.bin
    etopo1_ice_g_i2_s9.bin  > The website is showing etopo2v2c ?? I think this is an old version, assuming?

Create a link for topography folder:
    $cd ~/anaconda3
    $ln -s /home/adcpcode/topog .

- Download Smith Sandwell Topography V18.1 <ftp://topex.ucsd.edu/pub/global_topo_1min/topo_18.1.img>



## On Terminal

Navigate to:
    $cd ~/Downloads

Make a direcotory before moving topo v18.1: 
    $mkdir /home/adcpcode/topog/sstopo

Move smith-sandwell file topo18.1 into the sstopo folder:
    $mv topo_18.1.img /home/adcpcode/topog/sstopo

Install SSTOPO v18.1:
Navigate to:
    $cd /home/adcpcode/programs/pycurrents/scripts
	
Run:
    python topo_sub.py 
    > This will generate 2 more files for each topography folder. ** *This will take a long time!!* **

    
    










