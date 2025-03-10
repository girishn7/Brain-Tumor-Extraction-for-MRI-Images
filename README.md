# Brain-Tumor-Extraction-for-MRI-Images

Tumor Detection Using Anisotropic Diffusion and Morphological Operations
Overview
This MATLAB project implements a tumor detection algorithm that processes an input image (e.g., a medical image such as an MRI or CT scan) to identify and highlight potential tumor regions. The pipeline includes noise removal using anisotropic diffusion, thresholding, morphological operations, and visualization of the detected tumor with a bounding box and outline.

The code consists of two main parts:

anisodiff.m: A function that applies anisotropic diffusion to remove noise from the input image while preserving edges.
Main Script: A script that loads an image, applies the filter, performs tumor detection, and visualizes the results.
Dependencies
MATLAB: The code is written in MATLAB and requires a working installation.
Image Processing Toolbox: Functions like imfilter, imread, imshow, imfill, bwlabel, regionprops, etc., are used, which are part of the MATLAB Image Processing Toolbox.
Files
anisodiff.m: The anisotropic diffusion function.
Main Script: The tumor detection and visualization pipeline (unnamed in the provided code).
How It Works
1. Anisotropic Diffusion Function (anisodiff.m)
The anisodiff function applies an anisotropic diffusion filter to the input image to reduce noise while preserving important edges. This is particularly useful in medical imaging to enhance the quality of the image before further processing.

Algorithm
Converts the input image to double precision.
Defines gradient kernels (hN, hS, hE, hW, hNE, hSE, hSW, hNW) to compute differences in eight directions (North, South, East, West, and diagonals).
Iterates num_iter times:
Computes gradients in all eight directions using imfilter.
Calculates diffusion coefficients (cN, cS, etc.) based on the chosen option:
option == 1: Exponential function: exp(-(gradient/kappa)^2).
option == 2: Inverse quadratic function: 1/(1 + (gradient/kappa)^2).
Updates the image using the diffusion equation, incorporating the coefficients and gradients.
Returns the noise-reduced image.
2. Main Tumor Detection Script
The main script processes an input image to detect a tumor and visualizes the results in multiple steps.

Steps
Input Image Loading:
Prompts the user to select an image file (e.g., tumor.jpg) using a file dialog.
Reads the image using imread and displays it.
Noise Removal:
Applies the anisodiff function with predefined parameters:
num_iter = 10: 10 iterations of diffusion.
delta_t = 1/7: Time step for diffusion.
kappa = 15: Edge sensitivity parameter.
option = 2: Uses the inverse quadratic diffusion function.
Converts the filtered image to uint8 and resizes it to 256x256 pixels.
If the image is RGB, converts it to grayscale using rgb2gray.
Displays the filtered image.
Thresholding:
Computes a threshold th based on the mean pixel value of the original image and the average of its min and max values.
Applies binary thresholding to create a black-and-white image (sout):
Pixels above th are set to 1 (white).
Pixels below th are set to 0 (black).
Morphological Operations:
Labels connected components in the binary image using bwlabel.
Computes region properties (Solidity, Area, BoundingBox) using regionprops.
Identifies high-density regions (Solidity > 0.7) and finds the region with the largest area.
If the largest area exceeds 200 pixels, isolates the corresponding region as the tumor; otherwise, displays a "No Tumor" message and exits.
Bounding Box:
Extracts the bounding box of the detected tumor region and overlays it on the filtered image in yellow.
Tumor Outline:
Fills holes in the tumor region using imfill.
Erodes the filled image by 5 pixels to shrink the tumor boundary.
Subtracts the eroded image from the original tumor region to obtain the outline.
Displays the eroded image and the tumor outline.
Outline Overlay:
Creates an RGB version of the filtered image.
Colors the tumor outline red by setting the red channel to 255 and green/blue channels to 0 at outline pixels.
Displays the result with the red outline.
Final Visualization:
Displays six subplots showing:
Input image.
Filtered image.
Filtered image with bounding box.
Tumor alone.
Tumor outline.
Detected tumor with red outline.
