# Installing PrepSpec on MacOS

## Step 0: Install a fortran compiler of your choice
This is the crux of the issue (besides compiling) for getting PrepSpec to work. Much of the problems comes with your system (i.e., the OS and the architecture). It's particularly bad for me because I have a new Mac with an ARM64 architecture, but if you've got an x86_64 architecture you've got a load of options. I'll list a bunch of compilers here to look into:
* ``ifort``
* ``gfortran``
* ``flang-new``
* ``nagfor``
* ``lfortran``

I've only found both ``gfortran`` and ``nagfor`` to work on an ARM64 Mac, so I'll be sticking with ``gfortran``. You can usually install these compilers through ``homebrew`` or ``macports``, but sometimes a source installation may be necessary.


## Step 0.5: Install X11
PrepSpec relies on X11 to output its plots, so you'll need to install it if you don't have it already. New Macs have it installed already, but you can get it through a number of ways:
* ``homebrew``
* ``macports``
* https://www.xquartz.org/

It should be fine to install multiple versions of X11/Xquartz, though it did ruin my installation once so be weary.



## Step 1: Install PGPLOT
There are a couple of different ways of doing this on MacOS. See the following webpages:
* https://guaix.fis.ucm.es/~ncl/howto/howto-pgplot
* https://sites.astro.caltech.edu/~tjp/pgplot/macos/
* https://github.com/kazuakiyama/homebrew-pgplot

Since it's easiest, I'll be installing using MacPorts.

### Step 1.1: Install Homebrew
https://www.macports.org/install.php

### Step 1.3: Install PGPLOT
```
sudo port install pgplot
```

### Step 1.4: Set environmental variables
This may be different depending on the type of shell you use and/or your version of MacOS. On Ventura, using zsh, I had to alter my ``~/.zprofile`` file.
You need to set the following variables:
```
export PGPLOT_DIR=/opt/local/lib
export PGPLOT_DEV=/Xserve
```

### Step 1.5: Test PGPLOT
PGPGLOT has a number of demo scripts it makes, which you can use to test if it's working. I used MacPorts to install, so all of the demo files are located in ``$PGPLOT_DIR/examples``, and will be named ``pgdemoN``, where N is an integer.





## Step 3: Install PrepSpec

### Step 3.1: Get PrepSpec files
```
wget http://star-www.st-and.ac.uk/~kdh1/lib/prepspec/prepspec.tar.gz
```
__NOTE__: You can use Homebrew to install ``wget``.


### Step 3.2: Unzip
```
tar -xvf prepspec.tar.gz
```

### Step 3.3 Compile
This is __the__ hardest part. Each compiler has different options it needs. I'll just be listing the ones I found to work for ``gfortran``.

Firstly, I found that the libraries it links to can get messed up if your PATH directories are in the wrong order. I put this in my ``~/.zprofile`` file to fix this:
```
export PATH="/usr/bin/:/usr/local/bin:$PATH"
```

Now, there are different options for the library linking steps depending on where X11 is installed. These commands will all output an executable ``prepspec.sh`` to run. There may also be other options you want to use to compile (maybe to parallelize better or run quicker). This is what i've found to work, so experiment at your own risk (this could take a while). 

The only fancy thing I've done is to compile with OpenMP to make things quicker. For inspiration with other compilers, you can try using my examples or looking at the ``prepspec.com`` file.


#### Option A: Apple X11
```
gfortran -mdynamic-no-pic -O3 -fallow-argument-mismatch -fno-automatic -ffixed-line-length-132 -fopenmp -o prepspec.sh prepspec.for misc.for -L/opt/X11/lib -lX11 -L/opt/local/share/pgplot -L/opt/local/lib -lpgplot -Wl,-headerpad_max_install_names -L/opt/local/lib -Wl,-rpath,/opt/local/lib/libgcc
```


#### Option B: MacPorts X11
```
gfortran -mdynamic-no-pic -O3 -fallow-argument-mismatch -fno-automatic -ffixed-line-length-132 —fopenmp -o prepspec.sh prepspec.for misc.for -L/opt/local/share/pgplot -L/opt/local/lib -lpgplot -Wl,-headerpad_max_install_names -L/opt/local/lib -Wl,-rpath,/opt/local/lib/libgcc -L/opt/local/lib -lX11
```
