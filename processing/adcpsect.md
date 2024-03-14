# USING ADCPSECT  CODAS 
- I m using Ubuntu 16.04

## Step 1:
- Run ``quick_web.py --interactive`` to select your profile
- select sections and close figure
- the output will be in ~/os75nb/webpy

## Step 2: Decimal year to date
- Navigate to ~/os75nb/webpy
- Type: ``vi sectinfo.txt``
- Write down the values:
    - Section 1 that you selected during the step before = First line
    - Section 2  = Second line
- Type: ``:wq!`` to close editor
- Type: ``to_date yearbase decimalyearoffirstsection``
    - Eg: to_date 2016 291.3780637
    - You should see something like: 2016/10/18  09:04:24.70
    - Do the same with the other values from ``sectinfo.txt``

## Step 3: Edit timegrid
- Navigate to: ``~/os75nb/grid``
- Type: ``vi timegrid.cnt``
- edit the file with your dates. You should do something like:

    ```
    -----------------------------------------------------------------------------*/
        
        output: b329.tmg
        time_interval: 60
        time_range:
            2016/10/18  09:04:24.70 to 2016/10/19  03:27:33.67/* bm-to-SS#2 */
            2016/10/22  08:40:47.38 to 2016/10/22  22:00:34.39/* back to BM */
        
    /*****************************************************************************/     
    ```
- Save edits by typing ``:wq!``


## Step 4: Run timegrid
- on terminal, type `` timegrid timegrid.cnt``
- This will generate an output called ... .tmg
    - From my example it should generate b329.tmg
- Now you have the time range from all your edited sections


## Step 5: Editing adcpsect.txt
- Navigate to: ``~/os75nb/contour``
- Type:

    ```
    vi adcpsect.cnt
    :128,$d    
    ```
    This will delete all the lines from 128 to the end of the file
    
- On line 103, change the dname to:
    ```
    dbname: /home/fcp/Desktop/ADCP/done/10329/proc_10329/os75nb/adcpdb/a_ae
    ```
    - You need to change your directories to ~adcpdb/nameofyourfiles
- change the ``n_depth`` and ``yearbase`` and ``output name`` with your own datebase
- Type: ``:wq!`` to save edits and close the file

## Step 5: Cp and paste lines from .tmg (timegrid)
- Navigate to ``~/os75nb/grid``
- Open your .tmg  file (b329.tmg on my case)
- Type ``:%y`` to select all the lines from that file
- Type ``:n ../contour/adcpsect.cnt``. this will open the control file...
- Type ``:127gg`` to navigate to line 127
- Type ``:p`` to paste all contents from .tmg file
- Type ``:wq!`` to sabe and close the file

- Nice! Now you are ready to run adcpsect

    

## Step 6: Run adcpsect
- Navigate to ``~/os75nb/contour``
- type: ``adcpsect adcpsect.cnt`` 
- This should generate 4 files
    ```
    .con
    .sta
    _uv.mat
    _xy.mat
    ```







