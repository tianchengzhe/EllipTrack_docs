.. include:: .special.rst

.. _parameters_prob_Page:

==================================
Prediction of Events (*prob_para*)
==================================

*  :blueitalic:`empty_prob` :gray:`(double | advanced parameter)` --- Probability of an event (0 to 1) if no training data is provided.
*  :reditalic:`mitosis_inference_option` :gray:`(string)` --- Method of mitosis inference.

   .. list-table::
      :widths: 1 3 3
      :header-rows: 1

      * - Option 
        - Mother needs to have high 

          probabilities of being mitotic?
        - Daughters need to have high 

          probabilities of being newly born?
      * - 'all' or 'both'
        - Yes
        - Yes
      * - 'before'
        - Yes
        - No
      * - 'after'
        - No
        - Yes
      * - 'none'
        - No
        - No    

   Probabilities of "No" events will be modified to 0.5, such that these events' states will not affect the probability of cell lineages.

*  :reditalic:`migration_option` :gray:`(string)` --- Method of migration probability calculation.

   .. list-table::
      :widths: 1 3 3
      :header-rows: 1

      * - Option 
        - Consider Migration Distance?
        - Consider Ellipse Similarity?
      * - 'similarity'
        - Yes
        - Yes
      * - 'distance'  
        - Yes
        - No

   For 'similarity', the migration probability is calculated by

   .. math::
      
        & P\left(D_{i,t}\rightarrow D_{j,t+1}\right) \\
        = & \frac{P_{sim}\left(D_{i,t}, D_{j,t+1}\right) P_{mig}\left(D_{i,t}, D_{j,t+1}\right)}{P_{sim}\left(D_{i,t}, D_{j,t+1}\right) P_{mig}\left(D_{i,t}, D_{j,t+1}\right) + \left[1-P_{sim}\left(D_{i,t}, D_{j,t+1}\right)\right]P_{nonmig}}

   where :math:`D_{i,t}` and :math:`D_{j,t+1}` refer to the i-th ellipse in Frame t and j-th ellipse in Frame t+1, respectively; 
   :math:`P_{sim}\left(D_{i,t}, D_{j,t+1}\right)` is the probability that :math:`D_{i,t}` and :math:`D_{j,t+1}` represent the same cell;
   :math:`P_{mig}\left(D_{i,t}, D_{j,t+1}\right)` is the probability that a cell migrates from the position of :math:`D_{i,t}` to the position of :math:`D_{j,t+1}` by random walk;
   and :math:`P_{nonmig}` is a constant null probability for migration.

   For 'distance', the migration probability equals to :math:`P_{mig}\left(D_{i,t}, D_{j,t+1}\right)`.

*  :reditalic:`migration_speed` :gray:`(double or string)` --- Method of migration speed inference.

   Defined as the standard deviation of random walk in one direction and one frame. Available options:

   .. list-table::
      :widths: 1 5
      :header-rows: 1

      * - Option 
        - Description
      * - 'global'
        - Inference of one migration speed for all cells. 
      * - 'time'
        - Time-dependent inference of migration speeds. 
      * - 'density'
        - Local cell density-dependent inference of migration speeds.
      * - Number
        - A custom migration speed for all cells.

*  :blueitalic:`max_migration_dist_fold` :gray:`(double | advanced parameter)` --- Maximal distance (in folds of the migration speed) a cell can migrate in a frame.
*  :blueitalic:`migration_inference_resolution` :gray:`(double | advanced parameter)` --- 'time' and 'density' only. Resolution of time (in frames) or cell density (in the number of cells) for migration speed inference.
*  :blueitalic:`migration_inference_min_samples` :gray:`(integer | advanced parameter)` --- 'time' and 'density' only. Minimal number of training samples at each time/cell density for migration speed inference.
*  :blueitalic:`prob_nonmigration` :gray:`(double | advanced parameter)` --- Null probability (0 to 1) of migration.
*  :blueitalic:`min_inout_prob` :gray:`(double | advanced parameter)` --- Minimal probability (0 to 1) to migrate in/out of the field of view.
*  :blueitalic:`max_migration_time` :gray:`(integer | advanced parameter)` --- Maximal number of frames in a migration.

   *max_migration_time-1* equals to the maximal number of frames a track can skip at once. 
   
   Note that local track correction is not optimized for tracks skipping any frames. Error rates might be high.
