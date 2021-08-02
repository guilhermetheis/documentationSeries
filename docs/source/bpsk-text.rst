Creating a BPSK modulation with text file
=========================================

BPSK modulation with text file
------------------------------

You can refer to :ref:`bpsk-wav`, with some changes:

* The **Wav File Source** is replaced by **File Source** block
* Uses BCH encoder/decoder to transmit/receive the file data
* Has a **Throttle** block to limit the data's transfer rate
* Has a **BER** and **QT GUI Number Sink** to show the bit error rate
* The **Audio Sink** is replaced by **File Sink** block

The BCH encoder/decoder is available at *GitHub*, just follow installation instructions there.

https://github.com/lutgaru/gr-bchcoder

Now, for the configurations:

**File Source**:

.. list-table::
   :widths: 15 15 60
   :header-rows: 1

   * - Field
     - Type
     - Details
   * - ``File``
     - file_open
     - Path to file
   * - ``Ouput Type``
     - enum
     - byte
   * - ``Repeat``
     - enum
     - Repeat or not the file after ending
   * - ``Vec Length``
     - int
     - 1
   * - ``Add begin tag``
     - raw
     - pmt.PMT_NIL
   * - ``Offset``
     - int
     - 0
   * - ``Length``
     - int
     - 0

**bchencoder bb**:

.. list-table::
   :widths: 15 15 60
   :header-rows: 1

   * - Field
     - Type
     - Details
   * - ``Bch Type``
     - enum
     - BCH15_11 (3)

**Throttle**:

.. list-table::
   :widths: 15 15 60
   :header-rows: 1

   * - Field
     - Type
     - Details
   * - ``Type``
     - enum
     - byte
   * - ``Sample Rate``
     - float
     - samp_rate
   * - ``Vec Length``
     - int
     - 1
   * - ``Ignore rx_rate tag``
     - bool
     - True

**bchdecoder bb**:

.. list-table::
   :widths: 15 15 60
   :header-rows: 1

   * - Field
     - Type
     - Details
   * - ``Bch Type``
     - enum
     - BCH15_11 (3)

**File Sink**:

.. list-table::
   :widths: 15 15 60
   :header-rows: 1

   * - Field
     - Type
     - Details
   * - ``File``
     - file_save
     - Path to a file
   * - ``Input Type``
     - enum
     - byte
   * - ``Vec Length``
     - int
     - 1
   * - ``Unbuffered``
     - bool
     - False
   * - ``Append file``
     - bool
     - False

**BER**:

.. list-table::
   :widths: 15 15 60
   :header-rows: 1

   * - Field
     - Type
     - Details
   * - ``Test Mode``
     - enum
     - False
   * - ``BER Min. Errors``
     - int
     - 100
   * - ``BER Limit``
     - float
     - -7.0

**QT GUI Number Sink**:

.. list-table::
   :widths: 15 15 60
   :header-rows: 1

   * - Field
     - Type
     - Details
   * - ``Name``
     - string
     - "__"
   * - ``Input Type``
     - enum
     - float
   * - ``Autoscale``
     - enum
     - No
   * - ``Average``
     - float
     - 0
   * - ``Graph Type``
     - enum
     - Horizontal
   * - ``Number of Inputs``
     - int
     - 1
   * - ``Min``
     - float
     - -1
   * - ``Max``
     - float
     - 1
   * - ``Update Period``
     - float
     - 0.1
   * - ``GUI Hint``
     - gui_hint
     - *blank*

The flowgraph shoud look like this:

.. image:: /imgs/bpsk_wav/bpsk_text_1.png
