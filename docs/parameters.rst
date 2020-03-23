.. include:: .special.rst

.. _Parameters_Page:

==========
Parameters
==========

.. toctree::
   :hidden:
   :maxdepth: 2
   :titlesonly:
   
   parameters_movie_definition
   parameters_inout
   parameters_segmentation
   parameters_prob
   parameters_track_linking
   parameters_signal_extraction

Parameters are organized into six MATLAB structures (struct) as follows.

.. list-table::
   :widths: 1 3 3
   :header-rows: 1

   * - Structure
     - Description
     - Usage
   * - *movie_definition*
     - Movie specification
     - All steps
   * - *inout_para*
     - Locations of input and output files
     - All steps
   * - *segmentation_para*
     - Parameters for Segmentation
     - From Step 2 (Segmentation)
   * - *prob_para*
     - Parameters for predicting the 
   
       probabilities of events
     - From Step 4 
   
       (Prediction of Events)
   * - *track_para*
     - Parameters for Track Linking
     - From Step 5 (Track Linking)
   * - *signal_extraction_para*
     - Parameters for Signal Extraction
     - Step 7 (Signal Extraction)

Parameters within a structure can be accessed by the dot operator. For example, use ``movie_definition.image_type`` to access the parameter `image_type` within the structure `movie_definition`.

Parameters are further classified into two categories based on their importance to the tracking performance.

*  :redbold:`Basic`. Movie specification, input, outputs, and parameters with a critical impact on the tracking performance. 
*  :greenbold:`Advanced`. Parameters requiring an advanced understanding of the algorithm. Default values are often sufficient for a satisfactory tracking performance.
