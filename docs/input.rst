.. _Input_Page:

***********
Input Files
***********

Movies
######

Movies should be stored as image sequences, image stacks, or in Nikon ND2 formats.

Image Sequences
  Each file contains an image from one channel at one time point. All files from the same channel should locate in the same folder.
  
Image Stacks
  TIFF or GIF formats only. Each stack contains images from one channel at all time points. 
  
Nikon ND2 Format
  A movie can be stored in multiple segments (files). Each segment contains images of all channels at all time points within the segment. 
  This requirement is usually satisfied if the movie is captured by the Nikon NIS Element software.
  
Camera Dark Noise (Optional)
############################

Camera dark noise refers to the pixel intensities without illumination. Also known as CMOS Offset. 

Camera dark noise is represented as a MATLAB data file (.mat) with a single matrix called *cmosoffset*. 
*cmosoffset* has the same dimension as the movie images. Each element stores the value of camera dark noises at the corresponding pixel.

.. admonition:: Procedure of Generating *cmosoffset*

        * Capture a few images of an empty well with the laser switched off.
        * Load the images into MATLAB as a cell array called *all_images* where ``all_images{i}`` stores the i-th captured image.
        * Execute the following scripts
  
        .. code-block:: matlab

          cmosoffset = zeros(size(all_images{1})); % initialize the matrix
          for i=1:length(all_images) % adding the intensity values of each image
              cmosoffset = cmosoffset + double(all_images{i});
          end
          cmosoffset = cmosoffset / length(all_images); % average the intensities
          save('cmosoffset.mat', 'cmosoffset'); % save cmosoffset

Illumination Bias (Optional)
############################

Illumination bias refers to the uneven illumination of the lasers within the field of view. Correction of this bias is also known as shading correction.

Illumination bias is fluorescence channel-dependent. For each channel, it is represented as a MATLAB data file (.mat) with a single matrix called *bias*.
*bias* has the same dimension as the movie images. Each element stores the relative intensity at the corresponding pixel compared to the image average. 

.. admonition:: Procedure of Generating *bias*

        * Capture a few images of a well with medium only in the channel of interest.
        * Load the images into MATLAB as a cell array called *all_images* where ``all_images{i}`` stores the i-th captured image.
        * Execute the following scripts

        .. code-block:: matlab

          bias = zeros(size(all_images{1})); % initialize the matrix
              for i=1:length(all_images) % compute the bias from each image
                  temp = double(all_images{i}) - cmosoffset; % subtract camera dark noise
                  temp = temp / mean(temp(:)); % normalize the image
                  bias = bias + temp;
              end
          bias = bias / length(all_images); % average the bias
          save('bias.mat', 'bias'); % save the bias

Before analysis, EllipTrack removes the camera dark noises and illumination bias from each image as follows

.. code-block:: matlab

   I = (double(I) - cmosoffset) ./ bias;

Here, *I* refers to an image and ``./`` refers to elementwise-division. 
