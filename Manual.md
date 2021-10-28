# Myotube Analyzer Manual
This manual contains installation instructions and user guidelines for performing analyses with the Myotube Analyzer app. Technical details can be found in the publication about this app: (link)

## Installation
1. Download and run the installer for your operating system (Mac or Windows)
1. Allow the installer to make changes (Only on Windows)
1. Click 'Next'
1. Choose the installation folder (This is where log files appear if you do not opt for a desktop shortcut)
1. Install MATLAB runtime
1. Click 'Install'

The installer will now complete the installation, after which you can start the app by running the .exe file in the installation folder, or by using the desktop shortcut if you chose that option.

## Instructions

### 1. Image selection
Images used for analysis must be separate RGB channels in the JPG or PNG format. Microscope image files may be exported to these formats, and existing images in other formats may be mass-converted using tools like [XnConvert](https://www.xnview.com/en/xnconvert). To start the analysis, select an image for the red and blue channels. The red channel should contain myotubes, and the blue channel should contain nuclei. The green channel is optional and can be added later if necessary. It should contain a marker for nuclei (such as MyoD).

### 2. Adjusting image levels
The images selected for analysis need to be adjusted to improve contrast and visibility of structures. This step cannot be skipped, since the program only uses images created in this step for the other parts of the analysis. If your images have already been altered to improve their contrast and visibility, opening and saving the images with this function will allow you to continue to the next steps in the analyis as well.

To start, click the 'Adjust levels' button, then select which channel to adjust. The levels of the image can be adjusted using either the sliders or the text box values. To update the image, click the 'Adjust' button. For the best results, keep the lower input value as high as possible without removing structures, this will get rid of noise. The upper input value should be kept as low as possible without saturating the image, this will result in better contrast and visibility. Setting the upper input value first will make it easier to set the lower input value correctly. Once visibility and contrast are satisfactory, the adjusted image may be saved, and the process can be repeated for the other channels.

**Output:** The adjusted image of the selected channel, saved under the original name with the suffix '_adjusted'.

### 3. Creating/editing a mask
The mask created in this function is used to indicate which nuclei are inside myotubes, and to calculate metrics like the number of myotubes, number of branching points, myotube coverage and myotube diameter. This is the most important step of the analysis, and should be done carefully. Note that revisiting this step and changing the mask will require you to redo the steps after this one, since any changes do not automatically carry over to their outputs.

Click the 'Edit mask' button to start. If a mask does not exist already, the program will prompt you to select a threshold to create an initial mask. Set this threshold as low as possible without introducing noise (white speckles in the binary image). The mask created using the threshold can then be edited using the available functions. If a mask already exists, the program will load this mask and take the user straight to the editing functions.

#### Editing functions
* The **separating line** can be used to draw either black or white lines, depending on the value in the 'Remove' checkbox. To use the 'Undo', 'Back' and 'Done' buttons, first click the button and then draw a line in the image. The drawn line will be removed and the button will execute.
* **Edit area** works the same way, but drawn lines are closed to form an area instead of a line.
* **Fill holes** will fill any group of black pixels surrounded entirely by white pixels that does not touch the edge of the image.
* **Remove junk** removes any objects smaller than 50 pixels. You may want to use this function first to decrease computational load when editing, but make sure not to remove any important structures.
* **Reset mask** will reset to mask to the state it was in when the 'Edit mask' function was opened. If no mask existed yet, the mask first created using thresholding is the reset state.
* **Thresholding** allows you to redo the thresholding, essentially creating a new mask from scratch. Note that redoing thresholding will discard any unsaved changes you may have made ('Reset mask' will still work).
* **Back** takes the user back to function selection without saving anything
* **Save & exit** saves the mask and returns the user to function selection

#### Guidelines for mask editing
* Separate myotubes whenever a line or shadow runs over the whole length of the possible cut-off
* When myotubes overlap: keep the biggest myotube intact
* When in doubt, separate myotubes
* Dim myotubes that were not picked up by thresholding may be added if they contain at least 2 nuclei AND if 50% of the myotube was already in the mask
* Make sure to fill holes left by nuclei
* Always remove junk before saving

**Output:** a PNG image containing the mask, with different myotubes in a different colour. The file is named after the red channel image with suffix '_mask'.

### 4. Nuclei indication
The counting of nuclei allows for the calculation of fusion index, while the nuclei coordinates allow for clustering in the next step. If a green channel is present, this step will also count the number of nuclei that are positive for the marker in the channel.

If no output file exists yet, clicking 'Indicate nuclei' will prompt the user to enter the pixel size in µm. After that, the program provides an initial guess for the nuclei centres. If an output file already exists, the app will load the coordinate data from that file. Whichever the case, the app will bring up the editing functions, allowing the user to manually correct the indicated centres.

#### Editing functions
* **Remove nuclei** works the same way as the 'Edit area' function, and removes any nuclei inside the drawn areas
* **Add nuclei** allows the user to indicate nuclei centres: backspace to remove last, enter or double click to finish
* The **switch** determines which image is opened when adding/removing nuclei: left or right
* **Reset** brings the nuclei coordinates back to the state they were in when the function was opened
* **Redo analysis** brings back the initial guess for the nuclei centres
* **Back** takes the user back to function selection without saving anything
* **Save & exit** saves the nuclei coordinates and counts to the excel output file, and returns the user to function selection

#### Nuclei indication guidelines
* When in doubt whether a structure is a nucleus: remove it
* For poorly assigned centres (asterisk not centered but touching the border)
  * Less than 4 nuclei in the neighbourhood: leave as is, only add missing nuclei or remove excess
  * More than 4 nuclei in the neighbourhood: remove and reassign centres
  * When in doubt: remove and redo

**Output:** an Excel file with nuclei coordinates on the first page and nuclei counts on the second page, named after the blue channel image with suffix '_output'.

### 5. Nuclei clustering
This function uses the nuclei coordinates determined in the previous function to find nuclei clusters based on 3 parameters: number of neighbours, distance between nuclei and nucleus diameter.

After clicking the 'Nuclei clustering' button, the user is prompted to enter the nucleus diameter, and the maximal edge-to-edge distance between nuclei. Larger values will yield more and larger clusters. The program will then run the clustering algorithm with the parameters specified, calculate the trend line through each cluster and show a plot of all  clusters. Nuclei that do not belong to a cluster are red and marked as '-1'. All clusters have a positive number. Before saving, make sure to move the legend out of the figure. An image of the plot and legend is saved in the state it was in when telling the program to save.

**Output:** cluster stats on the 2nd page of the output excel file, cluster/trendline properties on the 3rd. A PNG image of the cluster plot, named after the blue channel image with suffix '_clusters'.

### 6. Branching points and diameter measurements

## Outputs
