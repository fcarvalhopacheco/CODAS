# Installing CODAS software on Anaconda3
- I am using Ubuntu 16.04


## Step 1:
- Download [Anaconda 3.6](https://www.anaconda.com/download/#linux)

- Follow instructions [here]( https://docs.anaconda.com/anaconda/install/linux)

- Check if anaconda installer have put a ``anaconda/bin`` subdirectory in your ``$PATH`` environment

**On Terminal:**
```python
# Navigate to
cd /home/your_username  

# Open your bash_profile
vi .bash_profile

# close the editor
:wq

```
You should be able to see something like
```python
# added by Anaconda3 installer
export PATH="/home/your_username/anaconda3/bin:$PATH"

```

If not, check on:
```python
vi .bashrc
```
Go to last line by typing:
```python
Shift+GG
```




## Step 2:
Create a Python 2.7 environment in your anaconda

  ```
  $conda create -n py27 python=2.7 anaconda
  ```

Activate Python 2.7

  ```
  $source activate py27
  ```


source activate py27  # to use spyder 2.7
----------------------------------------------
# (2) Anaconda - Packages
# Run the following codes with py27 activate
conda install basemap # Or conda install anaconda=custom basemap if conflict
conda install netcdf4
conda install wxpython=3 #Or more recent? conda install -c anaconda wxpython 
conda install future
-------------------------------------------
# (4) Mercurial
# https://currents.soest.hawaii.edu/docs/adcp_doc/codas_setup/anaconda_install/i# ndex.html
# The link from website seems to be broken http://mercurial.selenic.com/
sudo apt-get install mercurial

---------------------------------------------
# Aliases for wxpython on .bash_profile
alias dv='pythonw `which dataviewer.py` '
alias fv='pythonw `which figview.py` .'
alias gg='pythonw `which gautoedit.py` -n5'

---------------------------------------------
# Directories + clone 
sudo mkdir /home/adcpcode
sudo chown youruser:yourgroup /home/adcpcode

hg clone   http://currents.soest.hawaii.edu/hg/codas3          codas3
hg clone   http://currents.soest.hawaii.edu/hg/pycurrents      pycurrents
hg clone   http://currents.soest.hawaii.edu/hg/onship          onship
hg clone   http://currents.soest.hawaii.edu/hg/uhdas           uhdas
---------------------------------------------
# Compilling CODAS and Python extension
# Compile and install codas3
cd codas3 #on /home/adcpcode
./waf configure --python_env
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
