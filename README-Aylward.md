# Build new wheels for Windows

## Uninstall itk from current venv
    ./venv-311-x64/scripts/activate.bat
    pip list | grep itk
    pip uninstall itk itk-...

## Update code in ITK-Source/ITK
    cd ITK-Source/ITK
    git pull --all
    cd Module/Remote/TubeTK
    git pull --all

## Compile using VS command prompt
    python .\scripts\windows_build_wheels.py --py-envs "311-x64" --lib-paths .\oneTBB-prefix\bin --no-cleanup

# Setup Environment
Select paths are hard-coded in ITKPythonPackage.  These paths have been
updated in this version of ITKPythonPackage to match Stephen's build
environment.

## Install Doxygen in C:\P
Download installer from website and specify C:\P for destination

## Install Graphviz in C:\P
Download installer from website and specify C:\P for destination

## Install Python in C:\Python311-x64
Use install-python.ps1 script
