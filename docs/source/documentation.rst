Getting started with gnuradio 3.8 and rpitx/rtl-sdr
===================================================

First, install gnuradio

.. code-block:: bash

        sudo apt update
        sudo apt upgrade
        sudo apt install gnuradio

Then, to communicate to *rpitx* via **TCP** install the *gr-grnet* package which can be obtained here:

https://github.com/ghostop14/gr-grnet.git

Might be necessary to install *swig* and other packages to *cmake* run without errors.

.. note::

        The *tcp-server* block bundled with the gnuradio doesn't work (it refuses all connections with *rpitx* without a motive).
