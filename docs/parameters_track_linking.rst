.. include:: .special.rst

.. _parameters_track_linking_Page:

============================
Track Linking (*track_para*)
============================

Global Track Linking Algorithm
==============================

*  :greenitalic:`skip_penalty` :gray:`(double | advanced parameter)` --- Penalty score for skipping one frame.

   Score is calculated by ln(Prob) - ln(1-Prob).

*  :greenitalic:`multiple_cells_penalty` :gray:`(double | advanced parameter)` --- Penalty score for two tracks mapping to the same ellipse.
*  :greenitalic:`min_track_score` :gray:`(double | advanced parameter)` --- Minimal score of a track to be added to the cell lineages.
*  :greenitalic:`min_track_score_per_step` :gray:`(double | advanced parameter)` --- Minimal score of a track between two neighboring frames.

   Cell tracks with lower scores between any two neighboring frames will not be considered.

Local Track Correction (Post-Processing)
========================================

*  :greenitalic:`min_swap_score` :gray:`(double | advanced parameter)` --- Minimal gained score for alternative track configurations.

   Configurations with lower score gains will not be considered.

*  :greenitalic:`mitosis_detection_min_prob` :gray:`(double | advanced parameter)` --- Minimal probability (0 to 1) for mitosis detection.

   Mitosis events will be detected if both mother and daughters have probabilities greater than this value.

*  :greenitalic:`critical_length` :gray:`(integer | advanced parameter)` --- Critical length (in frames) for fixing mistakes related to under- and over-segmentation (Step 2).

   Suggested to be 10-20% of a typical cell cycle duration.

*  :reditalic:`min_track_length` :gray:`(integer)` --- Minimal length (in frames) of a valid track.

   Shorter tracks will be removed.

*  :reditalic:`max_num_frames_to_skip` :gray:`(integer)` --- Maximal number of frames a valid track can skip.

   Tracks skipping more frames will be removed.
   
