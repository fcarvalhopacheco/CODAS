# Installing CODAS software on Anaconda3
- I am using Ubuntu 16.04

## Step 1:
- Download Python 3.6 [Anaconda](https://www.anaconda.com/download/#linux).We will set up a python 2.7 environment

- Follow instructions [here]( https://docs.anaconda.com/anaconda/install/linux)

- Check if anaconda installer have put a ``anaconda/bin`` subdirectory in your ``$PATH`` environment

**On Terminal:**
```alias
 # Navigate to
 $cd /home/your_username  

 # Open your bash_profile
 $vi .bash_profile
```
You should be able to see something like

```
 # added by Anaconda3 installer
 export PATH="/home/your_username/anaconda3/bin:$PATH"

 # Close the editor
 :wq
```
If you can't find the ``$PATH`` on ``bash_profile``, check at the end of the ``.bashrc`` file:

```
 # Open bashrc
 $vi .bashrc

 # And type
 Shift+GG
```

## Step 2:
- Create a Python 2.7 environment in your anaconda

**On Terminal:**
```
 # Create py27
 $conda create -n py27 python=2.7 anaconda

 # Activate Python 2.7
 $source activate py27
```

## Step 3:
- Install Anaconda packages 

**On Terminal:**
```
 # Installing 4 packages

 $conda install basemap          # If conflict:  conda install anaconda=custom basemap 
 $conda install netcdf4          # For Ubuntu 14.04:  conda install -c conda-forge netcdf4   
 $conda install wxpython=3       # For most recent version: conda install wxpython
 $conda install future
```
---
## Step 4:
- Install Mercurial package 

**On Terminal:**
```
 $sudo apt-get install mercurial
```
> The link in the [website]( https://currents.soest.hawaii.edu/docs/adcp_doc/codas_setup/anaconda_install/index.html) seems to be broken
---
## Step 5:
- Create alias

**On Terminal:**
```
 # Navigate to:
 $cd /home/your_username

 # Open bash_profile
 $vi .bash_profile

 # Add the following aliases to your .bash_profile file
 alias dv="dataviewer.py "
 alias fv="figview.py"
 alias gg="gautoedit.py -n6"

 # Close editor
 :wq

 # Update your .bash_profile
 $source .bash_profile

 # activate python 2.7 again
 $source activate py27 
```

## Step 6:
- Get CODAS Mercurial Components

**On Terminal:**
```
 # Create directories and give permission to users
 $sudo mkdir /home/adcpcode
 $sudo chown youruser:yourgroup /home/adcpcode

 # Create subdirectories
 $mkdir /home/adcpcode/programs  # This is for mercurial repositories
 $mkdir /home/adcpcode/topog     # For topography plots 

 # Navigate to programs
 $cd /home/adcpcode/programs

 # Clone these (4) repositories as follows:
 $hg clone   http://currents.soest.hawaii.edu/hg/codas3          codas3
 $hg clone   http://currents.soest.hawaii.edu/hg/pycurrents      pycurrents
 $hg clone   http://currents.soest.hawaii.edu/hg/onship          onship
 $hg clone   http://currents.soest.hawaii.edu/hg/uhdas           uhdas
```

## Step 7:
- Compile CODAS and Python extension

**On Terminal:**
```
 # Navite to codas3
 $cd /home/adcpcode/programs/codas3

 # Compile and install codas3:
 $./waf configure --python_env
 $./waf build
 $./waf install
 $cd ..

 # For pycurrents
 $cd pycurrents
 $./runsetup.py
 $cd ..

 # Install uhdas and onship
 $cd uhdas 
 $./runsetup.py    
```
> This seems to be wrong on website runsetup.py insted of ./runsetup.py

```
 $cd ..
 $cd onship
 $python setup.py build
 $python setup.py install
 $cd..
```

## Step 8:
- Installing Topography

- Download [here](ftp://currents.soest.hawaii.edu/pub/outgoing/etopo1_for_pycurrents.zip)

- Unzip it into /home/adcpcode/topog/etopo

> The website is showing etopo2v2c ?? I think this is an old version, assuming?


```


# For Anaconda3
cd
cd anaconda3
ln -s /home/adcpcode/topog 

# For local linux
cd /usr/local
ln -s /home/adcpcode/topog
------------------------------------------------
# Update topography and smith sandwell
# download global topo 18.1 # ftp://topex.ucsd.edu/pub/global_topo_1min/topo_18.1.img 
# Create two directories inside topog in /home/adcpde/topog
mkdir sstopo # you should see etopo and sstopo in topog

# Move smith-sandwell file topo18.1 into the sstopo folder


--------------------------------------------- 
# Create a Python 3.5 env other than the root on Anaconda
conda create -n py36 python=3.6 anaconda

source activate py36

```
