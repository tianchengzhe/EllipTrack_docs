.. include:: .special.rst

.. _Outputs_Page:

=======
Outputs
=======

MAT Files
=========

All variables except *jitters* from Jitter Correction are m x n x p cell matrices, where ``variable_name{i,j,k}`` stores the information of the movie captured in Row i, Column j, and Site k of a multi-well plate.
For movies not captured on a multi-well plate, this information is stored in ``variable_name{1,1,1}``.

For clarity, only movie-specific information (``variable_name{i,j,k}``) will be described on this page. Data structure of ``variable_name{i,j,k}`` is shown after "|".
For example, :gray:`(m x n x p cell matrix | n x 1 cell array)` indicates that ``variable_name{i,j,k}`` is an n x 1 cell array.

Segmentation (*segmentation.mat*)
*********************************

*  *all_ellipse_info* :gray:`(m x n x p cell matrix | n x 1 cell array)` --- Information of the fitted ellipses.

   Each row of ``all_ellipse_info{i,j,k}`` is a MATLAB structure storing the information of one frame.

   .. code-block:: matlab

      >> all_ellipse_info{2,1,1}{50} % Row 2, Column 1, Site 1, Frame 50

      ans = 

         struct with fields:

            all_cartesian_para: {196×1 cell}
           all_parametric_para: {196×1 cell}
           all_boundary_points: {196×1 cell}
           all_internal_points: {196×1 cell}
                  all_features: {196×1 cell}

   All fields are n x 1 cell arrays, where each element stores the information of one ellipse.

   .. code-block:: matlab

      >> all_ellipse_info{2,1,1}{50}.all_parametric_para{100} % Row 2, Column 1, Site 1, Frame 50, Parametric Equation, Ellipse 100

      ans =

         1.0e+03 *

          0.0118
          0.0080
          1.0774
          0.7053
          0.0004

   Information for each ellipse
   
      *  *all_cartesian_para* :gray:`(nx1 cell array | 6x1 double array)` --- Fitted coefficients of the general ellipse equation 
     
         This array (denoted as :math:`a`) satisfies :math:`a_1 x^2 + a_2 xy + a_3 y^2 + a_4 x + a_5 y + a_6 = 0`.

      *  *all_parametric_para* :gray:`(nx1 cell array | 5x1 double array)` --- Fitted coefficients of the parametric ellipse equation.

         This array (denoted as :math:`a`) satisfies :math:`(x-a_3)^2/a_1^2 + (y-a_4)^2/a_2^2 = 1`. :math:`a_5` is the rotation angle.

      *  *all_boundary_points* :gray:`(nx1 cell array | 100x2 double matrix)` --- Coordinates of the points on the ellipse contour.

         Each row stores the coordinates of one point.
   
      *  *all_internal_points* :gray:`(nx1 cell array | mx2 integer matrix)` --- Coordinates of the pixels in the ellipse interior.

         Each row stores the coordinates of one pixel.

      *  *all_features* :gray:`(nx1 cell array | 26x1 double array)` --- Features of the ellipse.

         Each element stores the value of one feature.

*  *all_num_ellipses* :gray:`(m x n x p cell matrix | n x 1 integer array)` --- Number of ellipses in each frame.

   Each row ``all_num_ellipses{i,j,k}`` stores the number of ellipses in one frame.
   
   .. code-block:: matlab

      >> all_num_ellipses{2,1,1}(1:5) % Row 2, Column 1, Site 1, Frame 1-5

      ans =

         153
         153
         155
         158
         157

.. _outputs_mainfile_jitter_Page:

Jitter Correction (*jitter_correction.mat*)
*******************************************

*  *accumulated_jitters* :gray:`(m x n x p x 2 integer matrix)` --- Jitter values.

   For the movie captured in Row i and Column j, the jitters between Frame 1 and Frame t are stored in ``accumulated_jitters(i,j,t,:)``. 
   
   .. code-block:: matlab

      >> squeeze(accumulated_jitters(2,1,50,:)) % Row 2, Column 1, Between Frame 1 and Frame 50. No jitters.

      ans =

         0
         0
   
   EllipTrack assumes that movies captured in different imaging sites of the same well have the same jitter values.
   Jitters calculated from these movies will be averaged.

Prediction of Events (*probabilities.mat*)
******************************************

*  *all_morphology_prob* :gray:`(m x n x p cell matrix | n x 1 cell array)` --- Probabilities of morphological events.

   Each row of ``all_morphology_prob{i,j,k}`` stores the probabilities of one frame. 
   For every frame, the probabilities are organized into an nx2 double matrix, where each row refers to one cell and each column refers to one event. 
   Events are organized in the following order: :training1:`No Cells`, :training2:`One Cell`, :training3:`Two Cells`, :training4:`Mitotic (Before M)`, :training5:`Newly Born (After M)`, and :training6:`Apoptosis`.

   .. code-block:: matlab

      >> all_morphology_prob{2,1,1}{50}(1:5, :) % Row 1, Column 1, Site 1, Frame 50, Ellipse 1-5

      ans =

         0.0000    1.0000    0.0000    0.5000    0.0031    0.0001
         0.0000    1.0000    0.0000    0.5000    0.0003    0.0001
         0.0000    1.0000    0.0000    0.5000    0.0003    0.0001
         0.0000    1.0000    0.0000    0.5000    0.1952    0.0001
         0.0000    1.0000    0.0000    0.5000    0.0000    0.0001

      %  0 Cell    1 Cell    2 Cells   Before M  After M   Apoptosis

*  *all_migration_sigma* :gray:`(m x n x p cell matrix | n x 1 cell array)` --- Migration speeds.

   Each row of ``all_migration_sigma{i,j,k}`` stores the migration speeds of all ellipses in one frame. 
   For each frame, the migration speeds are organized into an nx1 double array, where each row refers to one ellipse.

   .. code-block:: matlab

      >> all_migration_sigma{2,1,1}{50}(1:5) % Row 2, Column 1, Site 1, Frame 50, Ellipse 1-5

      ans =

         18.9891
         18.0409
         17.0909
         17.3621
         17.0909

*  *all_prob_migration* :gray:`(m x n x p cell matrix | n x 1 cell array)` --- Migration probabilities.

   Denote :math:`D_{m,s}` and :math:`D_{n,t}` as the m-th ellipse in Frame s and n-th ellipse in Frame t (s<t), respectively.
   The migration probability from :math:`D_{m,s}` to :math:`D_{n,t}` is stored in ``all_prob_migration{i,j,k}{t}{n, t-s}(m)``.

   .. code-block:: matlab
   
      >> all_prob_migration{2,1,1}{50}{20, 1}(19) % Row 2, Column 1, Site 1, D_{19, 49} -> D_{20, 50}

      ans =

         0.5186

*  *all_prob_inout_frame* :gray:`(m x n x p cell matrix | n x 1 cell array)` --- Probabilities of cells migrating in/out of the field of view.

   Each row of ``all_prob_inout_frame{i,j,k}`` stores the probabilities of all ellipses in one frame. 
   For each frame, the probabilities are organized into an nx1 double array, where each row refers to one ellipse.

   .. code-block:: matlab

      >> all_prob_inout_frame{2,1,1}{50}(1:5) % Row 2, Column 1, Site 1, Frame 50, Ellipse 1-5

      ans =

         0.0411
         0.0001
         0.0001
         0.0001
         0.0001

*  *all_motion_classifiers* :gray:`(m x n x p cell matrix)` --- Classifiers for ellipse similarity.

   A Linear Discriminant Analysis (LDA) classifier is constructed for each movie to predict whether two ellipses represent the same cell.

Track Linking (*tracks.mat*)
****************************

*  *all_tracks* :gray:`(m x n x p cell matrix | n x 1 cell array)` --- Information of cell tracks.

   Each row of ``all_tracks{i,j,k}`` is a MATLAB structure storing the information of one cell track.

   .. code-block:: matlab 

      >> all_tracks{2,1,1}{50} % Row 2, Column 1, Site 1, Track 50

      ans = 

      struct with fields:

                  current_id: [288×1 double]
          gap_to_previous_id: [288×1 double]
              gap_to_next_id: [288×1 double]
                if_apoptosis: [288×1 double]
                   daughters: {288×1 cell}

   All fields are nx1 arrays, where each element stores the information of one frame.
   
   .. code-block:: matlab

      >> all_tracks{2,1,1}{50}.current_id(1:5) % Row 2, Column 1, Site 1, Frame 50, Ellipse ID, Frame 1-5

      ans =

         50
         49
         49
         48
         47


   .. list-table::
      :widths: 1 2 1
      :header-rows: 1

      * - Field :gray:`(Data Structure)`
        - Description
        - Track Absence
      * - *current_id* 
      
          :gray:`(nx1 integer array)`
        - Ellipse ID at this frame.
        - NaN
      * - *gap_to_previous_id* 
          
          :gray:`(nx1 integer array)`
        - Gap (in frames) between the current ellipse  

          and the previous ellipse of the cell track
        - NaN
      * - *gap_to_next_id* 
          
          :gray:`(nx1 integer array)`
        - Gap (in frames) between the current ellipse  

          and the next ellipse of the cell track
        - NaN
      * - *if_apoptosis*

          :gray:`(nx1 binary array)`
        - Whether cells undergo apoptosis 
        
          at this frame
        - 0
      * - *daughters*

          :gray:`(nx1 cell array)`
        - Daughter Track IDs. 
        
          Empty if cells are not mitotic.
        - Empty

   The column "Track Absence" indicates the value if a cell track is not present at this frame.

Signal Extraction (*signals.mat*)
*********************************

*  *all_signals* :gray:`(m x n x p cell matrix | n x 1 cell array)` --- Signals of cell tracks.

   Each row of ``all_signals{i,j,k}`` is a MATLAB structure storing the signals of one cell track.

   .. code-block:: matlab

      >> all_signals{2,1,1}{50} % Row 2, Column 1, Site 1, Track 50

      ans = 

      struct with fields:

                        ellipse_id: [288×1 double]
             num_tracks_at_ellipse: [288×1 double]
                         daughters: {288×1 cell}
                      nuc_center_x: [288×1 double]
                      nuc_center_y: [288×1 double]
                    nuc_first_axis: [288×1 double]
                   nuc_second_axis: [288×1 double]
                          nuc_area: [288×1 double]
                         nuc_angle: [288×1 double]
                      H2B_nuc_mean: [288×1 double]
                H2B_nuc_percentile: [288×1 double]
                  H2B_nuc_variance: [288×1 double]
                     CDK2_nuc_mean: [288×1 double]
               CDK2_nuc_percentile: [288×1 double]
                 CDK2_nuc_variance: [288×1 double]
                CDK2_cytoring_mean: [288×1 double]
          CDK2_cytoring_percentile: [288×1 double]
            CDK2_cytoring_variance: [288×1 double]
              intensity_percentile: 75

   Most fields are nx1 arrays, where each element stores the information of one frame.
   The field *intensity_percentile* is an mx1 double array, where each element stores a percentile to measure.
   The fields *XXX_XXX_percentile* are nxm double matrices, where each row refers to one frame and each column refers to one percentile specified by *intensity_percentile*.

   .. code-block:: matlab

      >> all_signals{2,1,1}{50}.ellipse_id(1:5) % Row 2, Column 1, Site 1, Frame 50, Ellipse ID, Frame 1-5

      ans =

         50
         49
         49
         48
         47

      % same as all_tracks{2,1,1}{50}.current_id(1:5)   

   .. list-table::
      :widths: 1 2 1
      :header-rows: 1

      * - Field :gray:`(Data Structure)`
        - Description
        - Track Absence
      * - *ellipse_id*

          :gray:`(nx1 integer array)`
        - Ellipse ID at this frame.
        - NaN
      * - *num_tracks_at_ellipse*
         
          :gray:`(nx1 integer array)`
        - Number of cell tracks mapping to 

          this ellipse, including this track
        - NaN
      * - *daughters*

          :gray:`(nx1 cell array)`
        - Daughter Track IDs. 
        
          Empty if cells are not mitotic.
        - Empty
      * - *nuc_center_x*, *nuc_center_y*

          :gray:`(nx1 double array)`
        - Position of the ellipse centroid.
        - NaN
      * - *nuc_first_axis*, *nuc_second_axis*

          :gray:`(nx1 double array)`
        - Axis lengths of the fitted ellipse.   
        - NaN 
      * - *nuc_area*       

          :gray:`(nx1 double array)`
        - Area of the fitted ellipse.
        - NaN
      * - *nuc_angle*       

          :gray:`(nx1 double array)`
        - Rotation angle of the fitted ellipse.
        - NaN
      * - *XXX_XXX_mean*

          :gray:`(nx1 double array)`
        - Mean pixel intensity in  

          the region of interest.
        - NaN
      * - *XXX_XXX_percentile*

          :gray:`(nxm double matrix)`
        - Percentiles of the pixel intensities 

          in the region of interest.
        - NaN
      * - *XXX_XXX_variance*

          :gray:`(nx1 double array)`
        - Variance of the pixel intensities 

          in the region of interest.
        - NaN

   The column "Track Absence" indicates the value if a cell track is not present at this frame.

Optional Outputs
================

Mask, Ellipse Movies, and Vistrack Movies
*****************************************

.. list-table::
   :widths: 1 2 2
   :header-rows: 1

   * - Output
     - Description
     - Example
   * - Mask

     - Binarized images of the nuclear 
      
       channel, before ellipse fitting.

     - .. figure:: _static/images/outputs/mask.png
          :align: center 
      
   * - Ellipse Movie
     - Autoscaled images of the nuclear channel, 

       overlaid by the fitted ellipses.
     - .. figure:: _static/images/outputs/ellipse_movie.png
          :align: center 

   * - Vistrack Movie
     - Ellipse Movie, with the Cell Track IDs 

       plotted next to their respective ellipses.

       * Green ID: Migration, including 

         in/out of the field of view.
       * Blue ID: Mitosis.
       * Red ID: Apoptosis.

     - .. figure:: _static/images/outputs/vistrack.png
          :align: center 

Masks and Ellipse Movies can be used to evaluate the accuracy of segmentation before and after ellipse fitting, respectively. Vistrack Movies can be used to evaluate the accuracy of tracking linking.

All the output images from one movie will be stored in the same folder (*RowID_ColumnID_SiteID*). Each image will follow the naming convention *RowID_ColumnID_SiteID_Channel_FrameID.tif*.
For example, the output image for Frame 4 of the movie captured in Row 2, Column 1, and Site 1 will be stored in the folder *2_1_1* and named *2_1_1_CFP_4.tif* (assuming that the nuclear channel is CFP).

Segmentation Info
*****************

MAT files storing the information of the fitted ellipses in one frame. Required for constructing training datasets. Optional otherwise.

All the output files from one movie will be stored in the same folder (*RowID_ColumnID_SiteID*). Each image will follow the naming convention *RowID_ColumnID_SiteID_Channel_FrameID_segmentation.mat*.
For example, the output file for Frame 4 of the movie captured in Row 2, Column 1, and Site 1 will be stored in the folder *2_1_1* and named *2_1_1_CFP_4_segmentation.mat* (assuming that the nuclear channel is CFP).

