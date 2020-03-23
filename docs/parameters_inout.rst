.. include:: .special.rst

.. _parameters_inout_Page:

=================================
Inout/Output Files (*inout_para*)
=================================

*  :reditalic:`training_data_path` :gray:`(nx1 cell array)` --- Paths to the training datasets.

   Each row stores the path for one dataset. Use empty cell array ({}) if no training datasets are available.

*  :reditalic:`output_path` :gray:`(string)` -- Path to the folder storing the output MAT files.

Available optional outputs:

.. list-table::
   :widths: 1 3
   :header-rows: 1

   * - Optional Output
     - Suggested Usage
   * - Mask 
     - Evaluation of segmentation accuracy, before ellipse fitting.
   * - Ellipse Movie
     - Evaluation of segmentation accuracy, after ellipse fitting.
   * - Segmentation Info 
     - Construction of training datasets.
   * - Vistrack Movie
     - Evaluation of track accuracy.

*  :reditalic:`mask_path` :gray:`(string)` -- Path to the folder storing the mask.

   Use empty string ('') if not generating this output.

*  :reditalic:`ellipse_movie_path` :gray:`(string)` -- Path to the folder storing the 'ellipse movie'.

   Use empty string ('') if not generating this output.

*  :reditalic:`seg_info_path` :gray:`(string)` -- Path to the folder storing the 'segmentation info'.

   Use empty string ('') if not generating this output.

*  :reditalic:`vistrack_path` :gray:`(string)` -- Path to the folder storing the 'vistrack movie'.

   Use empty string ('') if not generating this output.
