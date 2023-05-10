# Installing PrepSpec on MacOS

## Step 0: Install ``ifort``

### Steps 0.1: Get ifort files
Go to 






## Step 1: Install PGPLOT
There are a couple of different ways of doing this on MacOS. See the following webpages:
* https://guaix.fis.ucm.es/~ncl/howto/howto-pgplot
* https://sites.astro.caltech.edu/~tjp/pgplot/macos/

Since it's easiest, I'll be installing using MacPorts.

### Step 1.1: Install MacPorts
https://www.macports.org/install.php

### Step 1.2: Install X11 (optional)
You may need to install X11 if it isn't already installed on your system. A good way to check this is to type ``xclock`` into your terminal and seeing if a clock pops up.
This can be done through MacPorts:
```
sudo port install xorg-server
```

Or if you want all Xorg products:
```
sudo port install xorg
```

__NOTE__: If you already have X11 (or XQuartz) installed and install it again through MacPorts, it may mess things up. I've had to revert to an old backup of mine because of this. :(


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
PGPGLOT has a number of demo scripts it makes, which you can use to test if it's working. I used MacPorts to install, so all of the demo files are located in ``/opt/local/share/pgplot/examples``, and will be named ``pgdemoN``, where N is an integer.


