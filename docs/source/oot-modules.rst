Creating an OOT Module and edit in Eclipse
==========================================

This section is based on the following links:

https://wiki.gnuradio.org/index.php/Guided_Tutorial_GNU_Radio_in_C%2B%2B
https://wiki.gnuradio.org/index.php/OutOfTreeModules
https://wiki.gnuradio.org/index.php/UsingEclipse

OOT Module using C++ example (General block)
--------------------------------------------

First, install all the required dependencies:

.. code-block:: bash

   sudo apt update
   sudo apt upgrade
   sudo apt install swig libboost-all-dev cmake-data liblog4cpp5-dev codes libitpp-dev libcppunit-dev

Then, create a directory in which you'll place the OOT module:

.. code-block:: bash

   mkdir Custom OOTs
   cd Custom\ OOTs/
   mkdir My\ new\ Mod
   cd My\ new\ Mod/

Now issue the following command in the terminal:

.. code-block:: bash

   gr_modtool newmod test

This will create the ``gr-test`` folder. Go into that folder and issue the following commands:

.. code-block:: bash

   cd gr-test/
   gr_modtool add my_test_mod
   Enter block type: general
   Language (python/cpp): cpp
   Please specify the copyright holder: user
   Enter valid argument list, including default arguments: bool myParam
   Add Python QA code? [Y/n] n
   Add C++ QA code? [Y/n] n

Now you sucessfully created the OOT module. To edit in Eclipse, create the debug and release folders (alongside ``gr-test``):

.. code-block:: bash

   cd ../
   mkdir gr-test-build
   mkdir gr-test-release

Now do the following:

.. code-block:: bash

   cd gr-test-build/
   cmake -G "Eclipse CDT4 - Unix Makefiles" -D CMAKE_BUILD_TYPE=Debug ../gr-test/
   cd ../gr-test-release/
   cmake -G "Eclipse CDT4 - Unix Makefiles" ../gr-test/

You can now import the debug and release projects using Eclipse. When done editing, to install the block into GNU Radio:

.. code-block:: bash

   cd ../
   cd gr-test/
   mkdir build
   cd build/
   cmake ../
   make
   sudo make install
   sudo make ldconfig
