.. include:: .special.rst

.. _parameters_segmentation_Page:

==================================
Segmentation (*segmentation_para*)
==================================

Non-Specific Parameters (*nonspecific_para*)
============================================

*  :reditalic:`nuc_radius` :gray:`(integer)` --- Average radius (in pixels) of a nucleus.

*  :reditalic:`allowed_nuc_size` :gray:`(1x2 integer array)` --- Acceptable areas (in pixels) of a nucleus.

   Lower and upper limits. Mask components outside of this range will be removed.

*  :reditalic:`allowed_ellipse_size` :gray:`(1x2 integer array)` --- Acceptable areas (in pixels) of an ellipse.

   Lower and upper limits. Ellipses outside of this range will be removed.

*  :greenitalic:`max_ellipse_aspect_ratio` :gray:`(double | advanced parameter)` --- Maximal aspect ratio of an ellipse.

   Must be greater than 1. Ellipses with greater aspect ratios will be removed.

*  :greenitalic:`max_hole_size_to_fill` :gray:`(integer | advanced parameter)` --- Maximal area (in pixels) of a hole to fill.

   In masks, a hole is defined as a set of background pixels surrounded by foreground pixels. Holes with smaller areas will be converted to foreground pixels.

*  :greenitalic:`blur_radius` :gray:`(integer | advanced parameter)` --- Disk radius (in pixels) for image smoothing.

Image Binarization (*image_binarization_para*)
==============================================

*  :reditalic:`if_log` :gray:`(binary)` --- Whether to log-transform the images.

   .. list-table::
      :widths: 1 3
      :header-rows: 1

      * - Value
        - Suggested Situation
      * - 1
        - Nuclei have heterogeneous brightness.  
      * - 0
        - Nuclei have homogeneous brightness.  

*  :reditalic:`background_subtraction_method` :gray:`(string)` --- Method of Background Subtraction.

   .. list-table::
      :widths: 1 3
      :header-rows: 1

      * - Option
        - Description
      * - 'none'
        - No background subtraction is performed.
      * - 'min'
        - Minimal intensity of the image background is subtracted.
      * - 'median'
        - Median intensity of the image background is subtracted.
      * - 'mean'
        - Mean intensity of the image background is subtracted.

*  :reditalic:`binarization_method` :gray:`(string)` --- Method of Image Binarization.

   .. list-table::
      :widths: 1 2 2
      :header-rows: 1

      * - Option
        - Description
        - Suggested Situation
      * - 'threshold'
        - Thresholding. A threshold is applied 
         
          to the image intensities.
        - Nuclei have homogeneous 
         
          brightness.
      * - 'blob'
        - Blob Detection. A threshold is applied 
         
          to the hessian of image intensities.
        - Nuclei have heterogeneous 
      
          brightness.    

*  :reditalic:`blob_threshold` :gray:`(double)` --- Blob Detection only. Threshold of the hessian.

Active Contour (*active_contour_para*)
======================================

*  :reditalic:`if_run` :gray:`(binary)` --- Whether to run Active Contour.

   .. list-table::
      :widths: 1 5
      :header-rows: 1

      * - Value
        - Suggested Situation
      * - 1
        - Accurate nuclear boundaries have not been detected by Image Binarization.
      * - 0
        - Accurate nuclear boundaries have been detected by Image Binarization.  

   Setting the following parameters only if running Active Contour.

*  :reditalic:`if_log` :gray:`(binary)` --- Whether to log-transform the images.

   .. list-table::
      :widths: 1 3
      :header-rows: 1

      * - Value
        - Suggested Situation 
      * - 1
        - Nuclei have heterogeneous brightness.
      * - 0
        - Nuclei have homogeneous brightness. 

*  :reditalic:`active_contour_method` :gray:`(string)` --- Method of Active Contour

   .. list-table::
      :widths: 1 3 3
      :header-rows: 1

      * - Option 
        - Description
        - Suggested Situation
      * - 'local'
        - Applied to the neighborhood 
        
          of every nucleus.
        - Nuclei have heterogeneous brightness.
      * - 'global'
        - Applied to the entire image.
        - Nuclei have homogeneous brightness.

Watershed (*watershed_para*)
============================

*  :reditalic:`if_run` :gray:`(binary)` --- Whether to run Watershed.

   .. list-table::
      :widths: 1 3
      :header-rows: 1

      * - Value
        - Suggested Situation
      * - 1
        - Nuclei overlap frequently.
      * - 0
        - Nuclei do not overlap frequently.   

Ellipse Fitting (*ellipse_para*)
================================

The following descriptions are adapted from Zafari *et al.* 2015.

*  :greenitalic:`k` :gray:`(integer | advanced parameter)` --- Consider up to k-th adjacent points to the corner point.
*  :greenitalic:`thd1` :gray:`(integer | advanced parameter)` --- Distance (in pixels) between the ellipse centroid of the combined contour segments and the ellipse fitted to each segment.
*  :greenitalic:`thd2` :gray:`(integer | advanced parameter)` --- Distance (in pixels) between the centroids of ellipse fitted to each segment.
*  :greenitalic:`thdn` :gray:`(integer | advanced parameter)` --- Distance (in pixels) between contour center points.
*  :greenitalic:`C` :gray:`(double | advanced parameter)` --- Minimal aspect ratio for corner detection.

   Must be greater than 1. 
   
*  :greenitalic:`T_angle` :gray:`(integer | advanced parameter)` --- Maximal angle (in degrees) of a corner.
*  :greenitalic:`sig` :gray:`(integer | advanced parameter)` --- Standard deviation (in pixels) of the Gaussian filter.
*  :greenitalic:`Endpoint` :gray:`(binary | advanced parameter)` --- Whether to add the end points of a curve as corner.
*  :greenitalic:`Gap_size` :gray:`(integer | advanced parameter)` --- Maximal length of gaps (in pixels) in the contours to fill.

Correction with Training Data (*seg_correction_para*)
=====================================================

*  :reditalic:`if_run` :gray:`(binary)` --- Whether to run Correction with Training Data.

   .. list-table::
      :widths: 1 5
      :header-rows: 1

      * - Value
        - Suggested Situation
      * - 1
        - Training datasets are available and well-predict the 
        
          number of nuclei in each ellipse.
      * - 0
        - Training datasets are not available or not suitable.    

*  :greenitalic:`min_corr_prob` :gray:`(double | advanced parameter)` --- Minimal probability (0 to 1) for correction.
