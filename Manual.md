# Myotube Analyzer Manual
This manual contains installation instructions and intructions + guidelines for performing analyses with the Myotube Analyzer app. Technical details can be found in the publication about this app (to be published)

## Installation
1. Download and run the installer
1. Allow the installer to make changes
1. Click 'Next'
1. Choose the installation folder (This is where log files appear if you do not opt for a desktop shortcut)
1. Install MATLAB runtime
1. Click 'Install'

The installer will now complete the installation, after which you can start the app by running the .exe file in the installation folder, or by using the desktop shortcut if you chose that option.

## Instructions
This section explains all buttons and dials of the Myotube Analyzer app, in the order of a normal analysis procedure. The last part of this section gives an overview of all outputs of the app, and explains how to aggregate data from multiple image sets. The guidelines provided in this section are those used for the analyses in the publication about this app. You do not have to follow them to obtain usable results, feel free to determine your own guidelines. You are also free to use the images provided in this repository to test the functionality of the app.

### General
These are some general tips and rules that may help you when using the Myotube Analyzer:
* Save regularly! A single error or bug could ruin minutes of work, and all functions of the app support saving and reopening the analysis.
* Most functions have a statistics panel in the bottom left of the app, where you can keep track of the number of myotubes, number of nuclei, number of branching points...
* Most functions also have an image control panel, where you can open both the left and right image in a separate window, making it easier to zoom in/move around the image. This is especially useful when editing the mask. Having both the myotube image and the drawing figure side-by-side makes editing a lot easier.
* All images (both those in the app and those in separate windows) have buttons when you mouse over the top. They can be used to zoom in/out, move around the image, save the image (and the plot)...
* If you run into an error (evidenced by a sound effect and the app becoming unresponsive), restart the app. 

### 1. Image selection
Images used for analysis must be separate RGB channels in the JPG or PNG format. Microscope image files may be exported to these formats, and existing images in other formats may be mass-converted using tools like [XnConvert](https://www.xnview.com/en/xnconvert). To start the analysis, select an image for the red and blue channels. The red channel should contain myotubes, and the blue channel should contain nuclei. The green channel is optional and can be added later if necessary. It should contain a marker for nuclei (such as MyoD).

### 2. Adjusting image levels
The images selected for analysis need to be adjusted to improve contrast and visibility of structures. This step cannot be skipped, since the program only uses images created in this step for the other parts of the analysis. If your images have already been altered to improve their contrast and visibility, opening and saving the images with this function will allow you to continue to the next steps in the analyis as well.

To start, click the 'Adjust levels' button, then select which channel to adjust. The levels of the image can be adjusted using either the sliders or the text box values. To update the image, click the 'Adjust' button. For the best results, keep the lower input value as high as possible without removing structures, this will get rid of noise. The upper input value should be kept as low as possible without saturating the image, this will result in better contrast and visibility. Setting the upper input value first will make it easier to set the lower input value correctly. Once visibility and contrast are satisfactory, the adjusted image may be saved, and the process can be repeated for the other channels.

**Output:** The adjusted image of the selected channel, saved under the original name with the suffix '\_adjusted'.

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

**Output:** a PNG image containing the mask, with different myotubes in a different colour. The file is named after the red channel image with the suffix '\_mask'.

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
* For poorly assigned centres (asterisk is not centered, asterisks are missing, too many asterisks)
  * Less than 4 nuclei in the neighbourhood: leave as is, only add missing nuclei or remove excess
  * More than 4 nuclei in the neighbourhood: remove and reassign centres
  * When in doubt: remove and redo

**Output:** an Excel file with nuclei coordinates on the 2nd page and nuclei counts on the 1st page, named after the blue channel image with suffix '\_output'.

### 5. Nuclei clustering
This function uses the nuclei coordinates determined in the previous function to find nuclei clusters based on 3 parameters: number of neighbours, distance between nuclei and nucleus diameter.

After clicking the 'Nuclei clustering' button, the user is prompted to enter the nucleus diameter, and the maximal edge-to-edge distance between nuclei. Larger values will yield more and larger clusters. The minimal number of nuclei in a group is fixed at 4. The program will then run the clustering algorithm with the parameters specified, calculate the trend line through each cluster and show a plot of all clusters. Nuclei that do not belong to a cluster are red and marked as '-1'. All clusters have a positive number. Before saving, make sure to move the legend out of the figure. An image of the plot and legend is saved in the state it was in when telling the program to save.

**Output:** cluster stats on the 1st page of the output excel file, cluster/trendline properties on the 3rd. A PNG image of the cluster plot, named after the blue channel image with suffix '\_clusters'.

### 6. Branching
This function uses the myotube mask to determine myotube properties: number of branching points, myotube coverage and myotube diameter. The branching point analysis is based on the myotube skeleton, lines drawn along the centres of the myotubes.

Click 'Branching points' to start the function. If no branching point analysis was saved previously, the program will provide an initial guess based on the myotube skeletons. Else, the existing analysis is loaded. The program then brings up the editing functions.

#### Editing functions
* **Remove points** works the same way as the 'Edit area' function, and removes any branching points inside the drawn areas
* **Add points** allows the user to indicate branching points: backspace to remove last, enter or double click to finish
* **Redo** brings back the initial guess for the branching points
* **Min. branch length** specifies the shortest possible length for a skeleton branch, requires clicking 'redo' for changes to take effect
* **Find diameter** allows the user to sample points for a diameter measurement, explained below
* **Back** takes the user back to function selection without saving anything
* **Save & exit** saves the branching points and myotube statistics to the excel output file, and returns the user to function selection

#### Branching point guidelines
* Keep the general overview, do not zoom in
* If you cannot visually distinguish branching points, there should only be one
* Do not consider bulges for branching points, branches need to contain at least one nucleus (when in doubt, do not add a point)

#### Diameter measurements
Clicking 'Find diameter' opens a new figure where you can select points for a diameter measurement (same as the 'Add points' function). The image in the figure is the distance transform of the mask, where the intensity of each pixel is the distance to the closest black pixel. The diameter at each sampled point is calculated as this distance x2.

**Output:** statistics on the 1st page of the output file, myotube properties on the 4th page, branching point coordinates on the 5th page and (if applicable) diameter measurements on the 6th page. The image used for diameter measurements, carrying the name of the blue channel image with the suffix '\_diameters'.

## Outputs
The main strength of the Myotube Analyzer is that it has a lot of different outputs to allow you to come up with your own metrics or analyses. Not all of them were tested in the paper about the program. All numerical outputs are grouped in an Excel file named after the nucleus channel image with separate sheets for different parts of the analysis. Image outputs are in PNG format, named after either the nucleus or myotube channel. You can find examples of these outputs in the 'Example analysis' folder.

### The sheets of the Excel file in order

**1. General statistics**

![image](https://user-images.githubusercontent.com/62990029/146201949-c81626b0-4cf2-4cdb-b315-47aa0bc33bf2.png)

These can be output statistics (e.g. the total number of nuclei in the image) or input parameters (e.g. Max distance). The statistics are grouped by the function that calculated them: 'Nuclei indication' first, then 'Cluster nuclei', and 'Branching points' last. Pixel size is a user input for the indication of nuclei and is used in the calculation of several other parameters. Total nuclei is the total number of nuclei in the image. Nuclei in the mask is the total number of nuclei whose center is inside the MyHC mask. The fusion index is calculated as ``(Nuclei in mask)/(Total nuclei)``. + Nuclei is the number of nuclei with a center in the marker channel mask, while + Nuclei in mask is the number of nuclei with a center that is in both the MyHC and the marker channel mask. Number of clusters is the total number of clusters found (the 'cluster' containing all other nuclei is not counted). Nucleus diameter and Max distance are specified by the user before clustering. Average Rsq and Average RMSE are calculated from linear and orthogonal regression on the clusters, respectively. Branching points is the total number of branching points in the image. Myotubes is the number of separate objects in the MyHC mask. Points/myotube is calculated as ``(Branching points)/(Myotubes)``.

**2. Nucleus information**

![image](https://user-images.githubusercontent.com/62990029/146202595-1056d8d5-ebc8-4798-8c09-ee035758d71c.png)

This sheet contains image coordinates of individual nuclei (in pixels), as well as columns indicating whether they are a part of the MyHC and marker channel mask. The last column holds the label of the cluster that the nucleus belongs to. '-1' means the nucleus is not part of a cluster. The nuclei coordinates are used to load the saved analysis when reopening the 'Indicate nuclei' function.

**3. Clustering data**

![image](https://user-images.githubusercontent.com/62990029/146202730-403a78a0-a7f6-41da-94af-420fea0686a8.png)

The first column holds the labels corresponding to the clusters in the output image (see 'Output images'). The next three columns are the intercept, slope and linearity parameter determined using nuclei coordinates for linear regression, while the last three columns are the same parameters, but determined using orthogonal regression. It is important to note that Rsq is not a good linearity metric, since it is not independent of the orientation of the cluster. You may notice that while some clusters have a low RMSE value (good linearity), they also have a low Rsq (poor linearity). This is because Rsq goes to 0 for perfectly horizontal and vertical lines.

**4. Myotube data**

![image](https://user-images.githubusercontent.com/62990029/146202789-71601c6e-c484-46e7-a531-005ba10ffbc9.png)

This sheet is sorted from the largest to the smallest myotube. Total Area is calculated as the number of pixels that myotube occupies, divided by ``(pixel size)^2`` to obtain a value in µm. % of myotube area is the percentage of all MyHC-positive pixels that a myotube occupies. % of image area is the percentage of all image pixels that a myotube occupies.

**5. Branching point coordinates**

![image](https://user-images.githubusercontent.com/62990029/146203119-1cdad1ef-d88d-4d12-a306-107a7bc75cdb.png)

This sheet contains the coordinates of each branching point. It is used to load the saved analysis when reopening the 'Branching points' function.

**6. Diameter measurements**

![image](https://user-images.githubusercontent.com/62990029/146203187-a60e0344-6fd3-4462-b7f7-7b007b1c044c.png)

Separate measurements have their own header with the average and standard deviation, and are stacked below eachother. The measurements at individual points (and the coordinates of these points) are listed below the header, with the diameters calculated as ``((distance to the closest black pixel)*2)/(pixel size)``.

### Output images
The Myotube Analyzer outputs different images at different stages of the analysis. These images are saved in the same directory as the original images.

**1. Adjusted images**

![Example MyHC_Adjusted](https://user-images.githubusercontent.com/62990029/146204928-1ce0da89-f61a-4ff4-aa43-de2125f2daf4.png)

These images are the output of the 'Adjust levels' function, and are used in every further step of the analysis. Do not use these as an input in the file selector, they will not work. It is possible to create these images yourself using a different program, and save them with the '\_Adjusted' suffix so that the app can use them.

**2. Mask files**

![Example Dapi_labels](https://user-images.githubusercontent.com/62990029/146205815-63b2eff2-7b9d-4908-87ef-2180701d982c.png)

There are two versions of this image. One with labels (pure output, '\_labels') and one without (used by the program, '\_mask'). The mask file is create in the 'Edit mask' function, while the labelled version is created in the 'Branching points' function. Again, it is possible to create/edit the mask in a different program, so long as it bears the name of the myotube image with the suffix '\_mask'.

**3. Clustering image**

![Example Dapi_clusters](https://user-images.githubusercontent.com/62990029/146207477-94267a25-e857-47e8-94e8-2defbb2e1894.png)

This image shows the output of the clustering algorithm. Move the legend out of the way before saving the clustering analysis to ensure it does not block the image.

**4. Diameter measurements image**

![Example Dapi_Diameters](https://user-images.githubusercontent.com/62990029/146207858-09bac999-4b9f-40d1-9e8c-586e9ee46614.png)

This image is calculated using the distance transform on the myotube mask, which replaces any mask pixel by the distance to the closest black pixel. White pixels are far from the border of the myotube, while darker pixels are closer. The red line shows the myotube skeleton. This image is shown when selecting points for myotube diameter measurements.
