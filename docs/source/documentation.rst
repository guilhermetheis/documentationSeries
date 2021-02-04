Getting started with gnuradio 3.8 and rpitx/rtl-sdr
===================================================

First, install ``gnuradio``:

.. code-block:: bash

        sudo apt update
        sudo apt upgrade
        sudo apt install gnuradio

Then, to install ``rpitx`` go to the github and follow instructions there:

https://github.com/F5OEO/rpitx.git

To install the required software to make rtl-sdr working, there's some nice guides in this website:

https://ranous.wordpress.com/rtl-sdr4linux/

Now, to communicate to ``rpitx`` via *TCP*, install the ``gr-grnet`` package which can be obtained here:

https://github.com/ghostop14/gr-grnet.git

.. warning::

        Might be necessary to install ``swig`` and other packages to ``cmake`` run without errors. The install CAN be completed with ``cmake`` not finishing properly, but the gnuradio flowgraph will throw errors when trying to run!

.. note::

        The *tcp-server* block bundled with the gnuradio doesn't work (it refuses all connections with *rpitx* without a motive).

Doing these steps should be sufficient for making basic stuff with the hardware in conjunction with gnuradio.
