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
