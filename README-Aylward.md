# Build new wheels for Linux

## Prep

Join the docker group so that you can run docker without using sudo.

## Env Vars

set the following

    export ITK_MODULE_NO_CLEANUP=1

## Build ITK

Get scripts

    git clone https://github.com/aylward/ITKPythonPackage
    cd ITKPythonPackage
    git pull --all
    git checkout aylward20240807

* Or checkout whatever is the appropriate version

* Or checkout, update itkVersion, commit, push, and tag

    cd ITK-source/ITK
    git pull --all
    cd ../..
    vi itkVersion
    git checkout -b bump<date>
    git commit -a
    git tag aylward<date>
    git push aylward aylward<date>

On github site verify new tag.
    
Build ITK for cp310

    ./scripts/dockcross-manylinux-build-wheels.sh cp310

## Build dependent modules

    git clone https://github.com/InsightSoftwareConsortium/ITKMinimalPathExtraction

Don't forget to set the env variables and use the same python venv

Copy necessary directories from ITK

    cd ITKMinimalPathExtracction
    cp -r ../ITK-source .
    cp -r ../ITK-cp310* .
    cp -r ../oneTBB-prefix .
    cp -r ../dist .

### Set env variables

    export ITK_MODULE_NO_CLEANUP=1
    export TBB_DIR='/work/oneTBB-prefix/lib/cmake/TBB/'

### Update version

    vi pyproject.toml  # append date to version number

### Build it

    ../scripts/dockcross-manylinux-build-module-wheels.sh cp310

## Build TubeTK module

There must be a simpler way, but here is what I found that works...

### Copy build from ITKMinimalPathExtraction to ITKTubeTK

    cd src/wheels/ITKPythonPackage
    git clone https://github.com/InsightSoftwareConsortium/ITKTubeTK
    cp -r ITKMinimalPathExtraction ITKTubeTK
    cp -r ITKMinimalPathExtraction/dist/ ITKTubeTK
    cp dist/* ITKTubeTK/dist
    cp -r ITKMinimalPathExtraction/ITK-source ITKTubeTK
    cp -r ITKMinimalPathExtraction/oneTBB-prefix ITKTubeTK
    cp -r ITKMinimalPathExtraction/ITK-cp310* ITKTubeTK

### Set env vars

    cd ITKTubeTK
    export ITK_MODULE_NO_CLEANUP=1
    export TBB_DIR='/work/oneTBB-prefix/lib/cmake/TBB/'
    export ITK_MODULE_PREQ='InsightSoftwareConsortium/ITKMinimalPathExtraction@master'

### Update version

    vi CMakeLists.txt pyproject.toml

### Build it

    ../scripts/dockcross-manylinux-build-module-wheels.sh cp310

## Upload to TestPyPi

    twine upload --repository testpypi dist/*

# Build new wheels for Windows

## In Bash:

### Uninstall itk from current venv, using bash

    ./venv-311-x64/scripts/activate.bat
    pip list | grep itk
    pip uninstall itk itk-...

### Update code in ITK-Source/ITK

    cd ITK-source/ITK
    git pull --all

### Update verion

    cd /c/IPP
    vi itkVersion.py

## In VS Command Prompt

### Compile ITK using VS command prompt

    set_sys
    python .\scripts\windows_build_wheels.py --py-envs "311-x64" --no-cleanup

### Compile ITKMinimalPathExtraction using VS command prompt

    cd \IPP
    cd ITKMinimalPathExtraction
    git pull --all
    vimer pyproject.toml # Updated version to date
    python ..\scripts\windows_build_module_wheels.py  --py-envs "311-x64" --no-cleanup
    
### Compile ITKTubeTK using VS command prompt

    cd \IPP
    cd ITKTubeTK
    git pull --all
    vimer pyproject.toml CMakeLists.txt # Updated version to date in both
    python ..\scripts\windows_build_module_wheels.py  --py-envs "311-x64" --no-cleanup

### Upload to TestPyPi

    twine upload --repository testpypi dist/*

# Prerequisites: Setup Environment

Select paths are hard-coded in ITKPythonPackage.  These paths have been
updated in this version of ITKPythonPackage to match Stephen's build
environment.

## Install Doxygen in C:\P

Download installer from website and specify C:\P for destination

## Install Graphviz in C:\P

Download installer from website and specify C:\P for destination

## Install Python in C:\Python311-x64

Use install-python.ps1 script
