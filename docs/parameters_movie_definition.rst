.. include:: .special.rst

.. _parameters_movie_definition_Page:

=====================================
Movie Definition (*movie_definition*)
=====================================

*  :reditalic:`image_type` :gray:`(string)` --- Movie format. 

   .. list-table::
      :widths: 1 3
      :header-rows: 1

      * - Option
        - Movie Format
      * - 'seq'
        - Image Sequences (Individual Images)
      * - 'stack'
        - Image Stacks (Multi-Page Images)
      * - 'nd2'
        - Nikon ND2 Format

*  :reditalic:`image_path` :gray:`(nx1 cell array)` --- Path to the folder storing the images.

   Image Sequences/Stacks
     Each row stores the path to the folder for the i-th channel. 

     .. admonition:: Example
        :class: hint 
        
        **Example 1**. A movie has three channels: CFP, YFP, and mCherry. Images of these channels locate in ``X:/tracking_code/raw_images/CFP/``, ``X:/tracking_code/raw_images/YFP/``, and ``X:/tracking_code/raw_images/mCherry/``, respectively.

        .. code-block:: matlab

           image_path = {'X:/tracking_code/raw_images/CFP/';
                         'X:/tracking_code/raw_images/YFP/';
                         'X:/tracking_code/raw_images/mCherry/'};

        List the paths of different channels separately, even if all the images locate in the same folder.

        **Example 2**. A movie has 3 channels: CFP, YFP, and mCherry. All the images locate in ``X:/tracking_code/raw_images/``.

        .. code-block:: matlab 

           image_path = {'X:/tracking_code/raw_images/';
                         'X:/tracking_code/raw_images/';
                         'X:/tracking_code/raw_images/'};

        `image_path` should always be a cell array.

        **Example 3**. A movie only has the CFP channel. Images locate in ``X:/tracking_code/raw_images/``.

        .. code-block:: matlab

           image_path = {'X:/tracking_code/raw_images/'};

   Nikon ND2 Format
     Each row stores the path to the folder for the i-th segment.

     .. admonition:: Example
        :class: hint

        A movie is stored in two segments, locating in ``X:/tracking_code/raw_images/seg1/`` and ``X:/tracking_code/raw_images/seg2/``, respectively.

        .. code-block:: matlab

           image_path = {'X:/tracking_code/raw_images/seg1/';
                         'X:/tracking_code/raw_images/seg2/'};

.. admonition:: Formatting the Paths 

   For cross-platform compatibility, EllipTrack requires all paths to use forward slashes (/) rather than backward slashes (\\). 
   Paths to folders should end with a forward slash.

   Example 1. Path to a folder.
     **Incorrect format**. Use backward slashes rather than forward slashes; no forward slash at the end of the path.
     
     .. code-block:: matlab
    
        path = 'X:\tracking_code\raw_images';
    
     **Correct format**.

     .. code-block:: matlab

        path = 'X:/tracking_code/raw_images/';

   Example 2. Path to a MAT file.
     **Incorrect format**. Use backward slashes rather than forward slashes; use forward slash at the end of the path.

     .. code-block:: matlab

        path = 'X:\tracking_code\mat_files\cmosoffset.mat/';

     **Correct format**.

     .. code-block:: matlab

        path = 'X:/tracking_code/mat_files/cmosoffset.mat';
   
   EllipTrack automatically adjusts all paths to the correct format, though manual formatting is strongly suggested.

*  :reditalic:`filename_format` :gray:`(string)` -- Formats of image filenames.

   Filenames are specified with MATLAB FormatSpec_. Available conversion specifiers include

   .. list-table::
      :widths: 1 2 2
      :header-rows: 1

      * - Specifier
        - Data Type
        - Availability
      * - %r
        - Row ID, Number
        - All movie formats
      * - %a
        - Row ID, Letter (lower case)
        - All movie formats
      * - %b
        - Row ID, Letter (upper case)
        - All movie formats 
      * - %c 
        - Column ID, Number
        - All movie formats 
      * - %s 
        - Site ID, Number
        - Image sequences/stacks
      * - %i 
        - Channel ID, Number.
        - Image sequences/stacks 
      * - %n 
        - Channel Name, String.
        - Image sequences/stacks 
      * - %t 
        - Frame ID. Number.
        - Image sequences  

   Field widths can be specified. For example, ``%03t`` indicates that Frame ID has a minimal length of three digits, filled with prefix 0. This specifier will be converted to 001, 100, and 1000 for Frame 1, 100, and 1000.

   Full filenames should be specified for image sequences/stacks. A prefix is sufficient for the Nikon ND2 format.

   .. admonition:: Example
      :class: hint

      **Example 1 (Image Sequences)**. 
      The filenames follow the convention `RowID_ColumnID_SiteID_ChannelName_FrameID.tif`. Frame ID has a minimal length of three digits, filled with prefix 0. For example, `1_2_3_CFP_004.tif`.

      .. code-block:: matlab

         filename_format = '%r_%c_%s_%n_%03t.tif';

      **Example 2 (Nikon ND2 Format)**. 
      The filenames follow the convention `WellRowIDColumnID_CFP,YFP,mCherry_Seq00XX.nd2`. Row ID is an upper-case letter; Column ID is a two-digit number, filled with prefix 0; and *XX* represents an arbitrary number. For example, `WellA02_CFP,YFP,mCherry_Seq0001.nd2`.
      
      .. code-block:: matlab

         filename_format = 'Well%b%02c';

   Note that this parameter only defines the format of image filenames, not the actual values of Row ID, Frame ID, etc. These values will be defined by the parameters below.

*  :reditalic:`channel_names` :gray:`(nx1 cell array)` --- Name of the fluorescence channels

   Each row stores the name of the i-th channel. For image sequences/stacks, they must match the order of *image_path*. The first channel must image cell nuclei. 

*  :reditalic:`signal_names` :gray:`(nx1 cell array)` --- Names of the signals to measure

   Each row stores the name of the i-th signal. Must match the order of *channel_names*.

*  :reditalic:`if_compute_cytoring` :gray:`(nx1 binary array)` --- Whether to compute signals in the cytoplasmic ring.

   Each row is an indicator variable (1 for compute, 0 for not compute) for the i-th channel. Must match the order of *channel_names*. The first element must be 0.

*  :reditalic:`bias_paths` :gray:`(nx1 cell array)` --- Paths to the MAT files storing the illumination biases.

   Each row stores the path for the i-th channel. Must match the order of *channel_names*.

.. admonition:: Example
   :class: hint
   
   A movie has three channels: CFP, YFP, and mCherry, with the following information.

   .. list-table::
      :widths: 1 1 1 3
      :header-rows: 1

      * - Channel
        - Signal
        - Cyto Ring?
        - Path to Bias
      * - CFP
        - H2B
        - No
        - ``X:/tracking_code/bias/CFP.mat``
      * - YFP
        - CDK2 Sensor
        - Yes
        - ``X:/tracking_code/bias/YFP.mat``
      * - mCherry
        - ERK Sensor
        - Yes
        - ``X:/tracking_code/bias/mCherry.mat``

   .. code-block:: matlab

      channel_names = {'CFP';
                       'YFP';
                       'mCherry'};

      signal_names = {'H2B';
                      'CDK2';
                      'ERK'};

      if_compute_cytoring = [0;
                             1;
                             1];

      bias_paths = {'X:/tracking_code/bias/CFP.mat';
                    'X:/tracking_code/bias/YFP.mat';
                    'X:/tracking_code/bias/mCherry.mat'};

*  :reditalic:`cmosoffset_path` :gray:`(string)` --- Path to the MAT file storing the camera dark noises.

*  :reditalic:`wells_to_track` :gray:`(nx3 integer array)` --- Movie coordinates in a multi-well plate.

   Each row stores the Row, Column, and Site ID of one movie. Use [1, 1, 1] if the experiment is not performed in a multi-well plate.

*  :reditalic:`frames_to_track` :gray:`(integer array)` --- Frame IDs to track.

   Frame IDs do not need to be consecutive. Can start from zero.

*  :reditalic:`jitter_correction_method` :gray:`(string)` --- Method of Jitter Correction.

   .. list-table::
      :widths: 1 2 2
      :header-rows: 1

      * - Option
        - Description
        - Suggested Situation
      * - 'none'
        - No Jitter Correction is performed.
        - Jitters are negligible.
      * - 'local'
        - Jitters are calculated from each movie

          independently.
        - Movies are not captured 
        
          on the same multi-well plate. 
      * - 'global'
        - Jitters are calculated from the plate 
        
          motion jointly inferred from all movies.
        - Movies are captured on 
        
          the same multi-well plate. 

          Require at least 6 movies.

*  :reditalic:`num_cores` :gray:`(integer)` --- Number of local cores for parallel computing.

.. _FormatSpec: https://www.mathworks.com/help/matlab/ref/sprintf.html#btf_bfy-1_sep_shared-formatSpec