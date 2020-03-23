.. EllipTrack documentation master file, created by
   sphinx-quickstart on Tue May 21 21:40:26 2019.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

Welcome to EllipTrack's documentation!
======================================

.. toctree::
   :hidden:
   :maxdepth: 2
   
   input
   parameters
   GUI
   outputs
   tutorial

System Requirement
##################

Hardware
  A modern computer, at least 8GB RAM.

Software
  MATLAB (Mathworks), R2017a or later.

  Required toolboxes: System Identification Toolbox, Image Processing Toolbox, Statistics and Machine Learning Toolbox, Parallel Computing Toolbox, and MATLAB Distributed Computing Server.
  
  A C++ compiler in MATLAB with C++11 support. Install a free minGW compiler by following the instruction here_.

Installation
############

Download
********

Download the latest version from the `Github repository`_. 
Click "Clone or Download" button, and then click "Download ZIP". Extract files to the destination folder.

Compile *generate_tracks.cpp*
*****************************

*generate_tracks.cpp* is implemented in C++ for speed consideration. Compilation of this file is required before execution. Instruction:

*  Execute ``mex -setup`` to check whether a C++ compiler has been installed in MATLAB. Install a free minGW compiler (instruction here_) if not.
*  Navigate MATLAB to `functions/track_linking` folder.
*  Execute the following command in the command window.

   .. code-block:: matlab
   
      mex -largeArrayDims generate_tracks.cpp

*  Compilation succeeds if the following message appears

  ::

    MEX completed successfully.

  and file *generate_tracks.mexXXX* appears. 
  Here, *XXX* refers to the operating system (e.g. *w64* for 64-bit Windows, *maci64* for 64-bit Mac OS X, and *a64* for 64-bit Linux).


Install BioformatsImage
***********************

Required for analyzing Nikon ND2 files. Follow the `instruction here`_ for installation. Courtesy of Jian Wei Tay.

Overview of Files
#################

*  *mainfile.m*. Main script of EllipTrack. 
*  *parameters.m*. Parameters. 
*  *functions*. Customized scripts. Scripts are organized by their functions.
*  *GUI*. Parameter Generator GUI and Training Data GUI. 
*  *third_party_functions*. Scripts downloaded from Zafari *et al.* 2015 and MATLAB central.

MIT License
###########

Copyright (c) 2020 Chengzhe Tian, Chen Yang, Sabrina Leigh Spencer

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.

.. _here: https://www.mathworks.com/matlabcentral/answers/313290-how-do-i-install-mingw-for-use-in-matlab
.. _`instruction here`: https://biof-git.colorado.edu/core-code/bioformats-image-toolbox/wikis/home
.. _`Github repository`: https://github.com/tianchengzhe/EllipTrack
