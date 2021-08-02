Getting started with gnuradio 3.8 and rpitx/rtl-sdr
===================================================

First, install ``gnuradio``:

.. code-block:: bash

        sudo apt update
        sudo apt upgrade
        sudo apt install gnuradio

Then, install ``rpitx`` into the Raspberry Pi, following instructions there:

https://github.com/F5OEO/rpitx.git

To install the required software to make rtl-sdr working on linux, there's some nice guides in this website:

https://ranous.wordpress.com/rtl-sdr4linux/

Now, to communicate to ``rpitx`` via *TCP*, install the ``gr-grnet`` package which can be obtained here:

https://github.com/ghostop14/gr-grnet.git

.. warning::

        Might be necessary to install ``swig``, ``libpcap-dev`` and other packages to ``cmake`` run without errors. Also, you'll have
        to add the following into ``~/.bashrc``:

        export PYTHONPATH=/usr/local/lib/python3/dist-packages:/usr/local/lib/python3.9/site-packages:$PYTHONPATH

        export LD_LIBRARY_PATH=/usr/local/lib:$LD_LIBRARY_PATH

.. note::

        The python version that was used in the export was 3.9 at the time this guide was written.

.. note::

        The *tcp-server* block bundled with the gnuradio doesn't work (it refuses all connections with *rpitx* without a motive).

Doing these steps should be sufficient for making basic stuff with the hardware in conjunction with gnuradio.
