.. include:: .special.rst

.. _parameters_track_linking_Page:

============================
Track Linking (*track_para*)
============================

Global Track Linking Algorithm
==============================

All parameters are advanced. It is often unnecessary to modify their values.

*  :blueitalic:`skip_penalty` :gray:`(double | advanced parameter)` --- Score to subtract (>0) when cell tracks skip a frame.

   Score is calculated by ln(Prob) - ln(1-Prob). A positive (negative) score indicates that the probability of cell lineages increases (decreases).

*  :blueitalic:`multiple_cells_penalty` :gray:`(double | advanced parameter)` --- Score to subtract (>0) when two cell tracks map to the same ellipse.
*  :blueitalic:`min_track_score` :gray:`(double | advanced parameter)` --- Minimal score of a cell track (>0) to be added to the cell lineages.

   


   This score should be positive, such that only cell tracks increasing the probability of cell lineages will be included.

*  :blueitalic:`min_track_score_per_step` :gray:`(double | advanced parameter)` --- Minimal score of a cell track between two neighboring frames.

   Cell tracks with lower scores between any two neighboring frames will not be considered. 
   This score can be negative, such that cell tracks with occasional adverse probabilities between neighboring frames can still be considered.
   
Local Track Correction (Post-Processing)
========================================

*  :blueitalic:`min_swap_score` :gray:`(double | advanced parameter)` --- Step 1. Minimal score gain (>0) for alternative track configurations.

   Configurations with lower score gains will not be considered. 
   This value should be positive, such that only the significant mistakes of the global algorithm will be corrected. 

*  :blueitalic:`mitosis_detection_min_prob` :gray:`(double | advanced parameter)` --- Step 4. Minimal probability (0 to 1) for mitosis detection.

   Mitosis events will be detected if both mother and daughters have probabilities greater than this value.

*  :blueitalic:`critical_length` :gray:`(integer | advanced parameter)` --- Step 2. Critical length (in frames) for fixing mistakes related to under- and over-segmentation.

   Suggested to be 10-20% of a typical cell cycle duration.

*  :reditalic:`min_track_length` :gray:`(integer)` --- Step 5. Minimal length (in frames) of a valid cell track.

   Shorter cell tracks will be removed.

*  :reditalic:`max_num_frames_to_skip` :gray:`(integer)` --- Step 5. Maximal number of frames a valid cell track can skip.

   Cell tracks skipping more frames will be removed.
   
