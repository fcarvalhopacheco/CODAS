# Installing CODAS software on Anaconda3 (PYTHON2.7)
- I am using Ubuntu 16.04


## Step 1:
- Download [Anaconda 3.6](https://www.anaconda.com/download/#linux)

- Follow instructions [here]( https://docs.anaconda.com/anaconda/install/linux)

- Check if anaconda installer have put a ``anaconda/bin`` subdirectory in your ``$PATH`` environment

**On Terminal:**
```python
# Navigate to
`$`cd /home/your_username  

# Open your bash_profile
$vi .bash_profile

```
You should be able to see something like
```python
# added by Anaconda3 installer
export PATH="/home/your_username/anaconda3/bin:$PATH"

```
Close the editor

```python
:wq
```


If not, check at the end of the file:
```python
#open bashrc
$vi .bashrc

# Type
Shift+GG
```

## Step 2:
- Create a Python 2.7 environment in your anaconda

**On Terminal:**
```python
$conda create -n py27 python=2.7 anaconda

# Type yes to download and extract packages


# Activate Python 2.7
$source activate py27
```

## Step 3:
- Install Anaconda packages on with py27 activated

**On Terminal:**
```python

$conda install basemap          # Or conda install anaconda=custom basemap if you see any conflict
$conda install netcdf4
$conda install wxpython=3        
$conda install future
```
## Step 4:

- Install Mercurial package 

**On Terminal:**
```python
$sudo apt-get install mercurial
```
> The link in the [website]( https://currents.soest.hawaii.edu/docs/adcp_doc/codas_setup/anaconda_install/index.html) seems to be broken


## Step 5:

- Create alias

**On Terminal:**
```python
# Navite to:
$cd /home/your_username

# Open bash_profile
$vi .bash_profile

# Add the following aliases to your ``.bash_profile`` file
alias dv="dataviewer.py "
alias fv="figview.py"
alias gg="gautoedit.py -n6"

# Close editor
:wq

# Update your .bash_profile
$source .bash_profile

```
## Step 6:
- Get CODAS Mercurial Components

**On Terminal:**
```python

# Create directories and give permission to users
$sudo mkdir /home/adcpcode
$sudo chown youruser:yourgroup /home/adcpcode

# Create subdirectories
$mkdir /home/adcpcode/programs  # This for mercurial repositories
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
```python

# Navite to ``codas3``
$cd /home/adcpcode/programs/codas3

# Compile and install codas3:
$./waf configure --python_env
./waf build
./waf install
cd ..

# for pycurrents
cd pycurrents
./runsetup.py
cd ..

# install uhdas and onship
cd uhdas 
./runsetup.py     #this seems to be wrong on website runsetup.py insted of ./runsetup.py

cd onship
python setup.py build
python setup.py install
cd..
--------------------------------------------
# Getting CODAS non-Mercurial Components
# Installing Topography
# Download ftp://currents.soest.hawaii.edu/pub/outgoing/etopo1_for_pycurrents.zip
# Unzip it into /home/adcpcode/topog/etopo
# The website is showing etopo2v2c ?? but i think this is an old version, assuming? so just should see etopo1

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
