# How To Install PrepSpec
While I may not be a wizard, I can appear to be one after installing the illusive PrepSpec, and you can too! This is a step-by-step tutorial on how to install PrepSpec and (hopefully) begin running it yourself.
This is probably both system and OS dependent, but I'll give my method of doing so here. I've done this for CentOS (i.e., Linux) on one of the clusters here at UIUC. However, I don't have root or sudo access on the cluster, so I've installed everything locally. If you have sudo access, I'll try to make another tutorial. (But if I don't I guess this is the best you've got).


## Step 0: ``ifort``
This is an optional step, but one recommended by me and Keith. You can use the normal ``gfortran`` compiler to use PGPLOT and PrepSpec, but ``ifort`` is much quicker and easier to parallelize. I'll walk through the steps of installing ``ifort`` locally if you haven't already, If you're fine with using ``gfortran``, skip to Step 1.

## Step 1: PGPLOT
PrepSpec uses PGPLOT for its output and plotting functions, an old fortran-based software that I suspect isn't being updated anytime soon. Some useful websites for info on PGPLOT are:
* https://guaix.fis.ucm.es/~ncl/howto/howto-pgplot
* https://sites.astro.caltech.edu/~tjp/pgplot/

### 1.0 Activate ``ifort`` (optional)

### 1.1 Download PGPLOT files
```
wget ftp://ftp.astro.caltech.edu/pub/pgplot/pgplot5.2.tar.gz
```


### 1.2 Unzip
```
tar -xv pgplot5.2.tar.gz
```
You should have a PGPLOT directory: ``/path/to/cwd/pgplot``


### 1.3 Make a target directory for PGPLOT to be installed
```
mkdir pgplot_install
```

### 1.4 Copy and edit the ``drivers.list`` file
The ``drivers.list`` file located in the ``pgplot`` directory tells the install what functions it should compile. Both websites (and the file itself) contain descriptions of the different capabilities, and what is preferred to be installed. For PrepSpec, we only need:
* /GIF
* /VGIF
* /PS
* /VPS
* /VCPS
* /XSERVE

Copy the ``drivers.list`` file into the install directory:
```
cp pgplot/drivers.list pgplot_install
```

Edit the ``drivers.list`` file in the install directory, and remove the ``!`` in front of each of these titles




### 1.5 Making the Make file
In order to build PGPLOT, we need to create a Make file. We can do this in the ``pgplot_install`` directory with:
```
cd pgplot_install
/path/to/cwd/pgplot/makemake /path/to/cwd/pgplot/ linux g77_gcc
```
__NOTE__: This assumes that the ``gcc`` C++ compiler is installed



### 1.6 Editing the Make file
Now, we need to edit the Make file to use the Fortran compiler we want.
Go into ``makefile`` and edit the following declaration:
```
FCOMPL={compiler} 
```
where ``{compiler}`` is either ``gfortran`` or ``ifort``. (I've used ``gfortran`` in this case because using ``ifort`` gave me errors).


We also need to tell the Makefile to use the ``gfortran`` library (I don't know if this will be different if ``ifort`` is used). Add the ``-lgfortran`` option to the following declarations:
* LIBS
* FFLAGC
* FFLAGD
* PGPLOT_LIB
* CPGPLOT_LIB
* SHARED_LD

PGPLOT will only utilize ``libpgplot.so.0``, so change the ``libpgplot.so`` in SHARED_LIB and SHARED_LD to ``pibpgplot.so.0``.

### 1.7 Compile PGPLOT
Run the makefile:
```
make
make cpg
make clean
```
__NOTE__: You may need to point the install toward the ``gcc`` library. You can do this with ``export LD_LIBRARY_PATH=/usr/lib/gcc:$LD_LIBRARY_PATH``.


### 1.8 Set environmental variables
Now that we've installed PGPLOT, we need to set environmental variables to point towards our installation directory:
```
export PGPLOT_DIR=/usr/local/pgplot
export PGPLOT_DEV=/Xserve

export LD_LIBRARY_PATH=/path/to/cwd/pgplot:$LD_LIBRARY_PATH
export LD_LIBRARY_PATH=/path/to/cwd/pgplot_target:$LD_LIBRARY_PATH 
```
I've put these in my ``.bashrc`` file so I don't have to do this every time I open a terminal.


### 1.9 Test PGPLOT (optional)
We can now test that PGPLOT works using the demo scripts located in ``/path/to/cwd/pgplot_install``. These demo scripts will all be called ``pgdemo_`` and can be run with ``./pgdemo_``.

__NOTE__: If using a remote connection, you will need to create an X-server before connecting via SSH, and use the ``-X`` option when SSHing. I'd advise using the ``-Y`` option instead of ``-X``, as ``-X`` will disconnect the X-server after $\sim$ 20 min.





## Step 2: PrepSpec

### Step 2.0 Activate ``ifort``

### Step 2.1 Get PrepSpec source files
```
wget http://star-www.st-and.ac.uk/~kdh1/lib/prepspec/prepspec.tar.gz
```

### Step 2.2: Unzip
```
tar -xv prepspec.tar.gz
```
__NOTE__: This will place all files into a directory called ``export``, which I'll change to ``prepspec_dir``.


### Step 2.3: Run
This is where the different systems and OSs may also come into play. On CentOS, the executable file can be run automatically after activating``ifort``. Simply:
```
./prepspec.exe
```

