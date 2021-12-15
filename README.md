# Myotube Analyzer App
The Myotube Analyzer is an open source app made specifically for the analysis of microscopic images that capture cell cultures of muscle stem cells. The app was developed by Simon Noë, part of the 3D-MMAP research group at KU Leuven (Belgium). A paper about the app was published here: (link) A detailed user manual can be found in the 'Manual.md' file.

# Installation
1. Download and run the installer for your operating system (Mac or Windows)
1. Allow the installer to make changes (Only on Windows)
1. Click 'Next'
1. Choose the installation folder (This is where log files appear if you do not opt for a desktop shortcut)
1. Install MATLAB runtime
1. Click 'Install'

The installer will now complete the installation, after which you can start the app by running the .exe file in the installation folder, or by using the desktop shortcut if you chose that option.

# Changelog
## Myotube Analyzer v1.0
Released 29/03/21

## Myotube Analyzer v1.1
Released 21/04/21
- Added 'find diameters' function to branching point analysis
- Added stats window
- Cleaned up excel output, made separate sheets for analyses and added stats
- Added support for MyoD images
- Added pixel size
- Added trend line analysis of cluster
- Added the option to use existing coordinates for nuclei indication
- Fixed bug that occurred if user zoomed in while adding nuclei

## Myotube Analyzer v1.2
Released 23/04/21
- Level adjustment is now a separate function, needs to be used for each image before analysis
	- Saves adjusted version of images for later use
- Corrected 'satellite cells' to 'MyoD+ nuclei'
- Added number of MyoD+ nuclei inside the mask to output
- Indicate nuclei now always uses existing coordinates
- Added redo and reset buttons to nuclei indication
- Changed cluster distance to 14 µm
- Fixed bug that caused incorrect values in regression output
- Added orthogonal regression to the output
	- Low RMSE values indicate good linearity

## Myotube Analyzer v1.2.1
Released 27/04/21
- Saving cluster data when no clusters are present no longer gives an error
- Fixed a bug causing image scaling issues in the clustering function

## Myotube Analyzer v1.2.2
Released 01/05/21
- Fixed error in nuclei removal tool
- It is now possible to do the analysis with DAPI and MyHC, and add MyoD later

## Myotube Analyzer v1.2.3
Released 03/05/21
- Fixed an error in nuclei clustering

## Myotube Analyzer v1.3
Released 07/07/21
- Changed edit mask functions to draw multiple lines in one go
- Added the ability to undo previously drawn lines
- Added the ability to return to mask editing functions without changes
- Points used for diameter measurements no longer snap to the image skeleton
- Clustering parameters can now be specified before clustering

## Myotube Analyzer v1.4
Released 05/08/2021
- Updated 'Branching points' function to correspond to other functions
- Added a lot of 'back' buttons
- Added the ability to redo thresholding while editing the mask
- Made drawing in 'Remove nuclei' and 'Remove branching points' work the same way as in 'Edit area'
- Nuclei clusters now saves an image of the analysis
- Changed the image for indicating diameters to the distance transformed image with the skeleton overlaid

## Myotube Analyzer v1.5
Released 17/08/2021
- Previous Excel files will be incompatible
- Clustering now overwrites existing data
- The 'Branching points' function now saves coordinates, and loads previously saved coordinates
	- Also saves the area (in µm²) of each myotube, with labels
	- Saves a labeled version of the mask
- The image for the diameter indication is now saved after measurements
- Pixel size is only modifiable in the nuclei indication function, the program asks for it when a new analysis is started
- Added a panel with buttons to open figures in a separate window in compatible functions
- The 'Edit mask' function now reminds users to remove junk before saving
- Updated clustering function to produce statistics in µm, not pixels
- It is now possible to close figures used for drawing/adding points
- Rearranged Excel file, added extra statistics
- Added a 'back' button to thresholding
- Fixed a bug in the 'Remove junk' function

## Myotube Analyzer v1.5.1
Released 17/08/2021
- Changed default pixel size to 0.664
- Changed default minimum branch length to 100
- Clicking the undo/back/done button while drawing now brings up the figure
- Minimum branch length is now loaded from the excel file if possible

## Myotube Analyzer v1.5.2
Released 24/08/2021
- Added a check that removes branching points outside the mask
- Added the option to choose the right image for nuclei indication/removal

## Myotube Analyzer v1.5.3
Released 30/09/2021
- Swapped dimensions of branching point check to fix bug

## Myotube Analyzer v2.0 (Public release)
- Removed upper limits for adjust image function
- Added a check to see if limits are correct
- Removed DAPI, MyHC and MyoD labels
- Renamed Min distance to Max distance
