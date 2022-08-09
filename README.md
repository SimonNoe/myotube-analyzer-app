# Myotube Analyzer App
The Myotube Analyzer is an open source app made specifically for the analysis of microscopic images taken of muscle stem cell cultures. The app was developed for the 3D-MMAP research group at KU Leuven (Belgium). All technical details can be found in the [paper about the app](https://doi.org/10.1186/s13395-022-00297-6). A detailed user manual and an explanation of the outputs can be found in the 'Manual.md' file. An example of an analysis performed with the app can be found in the 'Example Analysis' folder.

# Installation
1. Download (click on 'MA_Installer.exe' and find the download button) and run the installer
1. Allow the installer to make changes
1. Click 'Next'
1. Choose the installation folder (This is where log files appear if you do not opt for a desktop shortcut)
1. Install MATLAB runtime
1. Click 'Install'

The installer will now complete the installation, after which you can start the app by running the .exe file in the installation folder, or by using the desktop shortcut if you chose that option.

# Changelog
## Version 1.0.1 (Released 09/08/2022)
* Added the number of nuclei per myotube
* Corrected a small error in the calculation of myotube surface area, all myotubes were very slightly underestimated
Re-running the branching points function in the new version on existing data is enough for both of these changes to take effect.