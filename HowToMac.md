# Installing PrepSpec on MacOS

## Step 0: Install ``ifort``

### Step 0.1: Get ``ifort`` files
https://www.intel.com/content/www/us/en/developer/articles/tool/oneapi-standalone-components.html#fortran

### Step 0.2: Setup ``ifort``
You can choose to enter this in every terminal you wish to use ``ifort`` in. I've put these in my ```~/.zprofile`` file to do this automatically.
```
source /opt/intel/oneapi/setvars.sh
```



## Step 1: Install PGPLOT
There are a couple of different ways of doing this on MacOS. See the following webpages:
* https://guaix.fis.ucm.es/~ncl/howto/howto-pgplot
* https://sites.astro.caltech.edu/~tjp/pgplot/macos/
* https://github.com/kazuakiyama/homebrew-pgplot

Since it's easiest, I'll be installing using Homebrew.

### Step 1.1: Install Homebrew
```
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

### Step 1.3: Install PGPLOT
```
brew install kazuakiyama/pgplot/pgplot
```
__NOTE__: You may need to install X11/XQuartz separately. this already came installed on my mac so i had no issues. You can do this through MacPorts or the XQuartz website.


### Step 1.4: Set environmental variables
This may be different depending on the type of shell you use and/or your version of MacOS. On Ventura, using zsh, I had to alter my ``~/.zprofile`` file.
You need to set the following variables:
```
PGPLOT_DIR=`brew --prefix pgplot`/share
if [ -e $PGPLOT_DIR ]; then
  export PGPLOT_DIR=$PGPLOT_DIR
  export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:$PGPLOT_DIR
fi

export PGPLOT_DEV=/Xserve
```

### Step 1.5: Test PGPLOT
PGPGLOT has a number of demo scripts it makes, which you can use to test if it's working. I used MacPorts to install, so all of the demo files are located in ``$PGPLOT_DIR/examples``, and will be named ``pgdemoN``, where N is an integer.





## Step 3: Install PrepSpec

## Step 3.1: Get PrepSpec files
```
wget http://star-www.st-and.ac.uk/~kdh1/lib/prepspec/prepspec.tar.gz
```
__NOTE__: You can use Homebrew to install ``wget``.


## Step 3.2: Unzip
```
tar -xvf prepspec.tar.gz
```

## Step 3.3 Compile
This is __the__ hardest part.

