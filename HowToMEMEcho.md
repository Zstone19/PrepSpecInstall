This is pretty self explanatory once you've set up PrepSpec.

# Step 1: Download files
```
wget http://star-www.st-and.ac.uk/~kdh1/lib/mem/echo/memecho.tar.gz
```

# Step 2: Unzip
```
tar -xvf memecho.tar.gz
```

# Step 3: Compile
For this, you should use the same command line code to compile MEMEcho as PrepSpec. Instead of ``-o prepspec.sh prepspec.for misc.for``, you should put ``-o memecho.sh memecho.for misc.for memsys.for``.
