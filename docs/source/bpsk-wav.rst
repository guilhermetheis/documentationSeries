.. _bpsk-wav:

Creating a BPSK modulation with audio
=====================================

BPSK modulation with a *wav* file
---------------------------------

First, place an *wav* audio file in the project's directory, then add the **Wav File Source** block with the following parameters:

.. list-table::
   :widths: 15 15 60
   :header-rows: 1

   * - Field
     - Type
     - Details
   * - ``File``
     - file_open
     - Path to a *wav* file
   * - ``Repeat``
     - enum
     - Repeat or not the audio file after ending
   * - ``N Channels``
     - int
     - 1

Then, use the **Float To Char** block to convert the wav data into char arrays:

.. list-table::
   :widths: 15 15 60
   :header-rows: 1

   * - Field
     - Type
     - Details
   * - ``Vec Length``
     - int
     - 1
   * - ``Scale``
     - float
     - 32

Now, to convert the raw data to BSPK modulation, use the **Repack Bits** with 8 bits per input byte and 1 bit per output byte, since the BPSK modulation will generate a pair of symbols mapped to (0,1):

.. list-table::
   :widths: 15 15 60
   :header-rows: 1

   * - Field
     - Type
     - Details
   * - ``Bits per input byte``
     - int
     - 8
   * - ``Bits per output byte``
     - int
     - 1
   * - ``Length Tag Key``
     - string
     - "__"
   * - ``Packet Alignment``
     - enum
     - Input
   * - ``Endianness``
     - int
     - LSB (1)

To effectively convert the individual bits to symbols, use the **Chunks to Symbols** block with the following parameters:

.. list-table::
   :widths: 15 15 60
   :header-rows: 1

   * - Field
     - Type
     - Details
   * - ``Input Type``
     - enum
     - byte
   * - ``Output Type``
     - enum
     - complex
   * - ``Symbol Table``
     - complex_vector
     - [0, 1]
   * - ``Dimention``
     - int
     - 1
   * - ``Num Ports``
     - int
     - 1

Finally, add the **Noise Source** block to generate some noise to the modulation:

.. list-table::
   :widths: 15 15 60
   :header-rows: 1

   * - Field
     - Type
     - Details
   * - ``Output Type``
     - enum
     - complex
   * - ``Noise Type``
     - int
     - Gaussian (201)
   * - ``Amplitude``
     - float
     - 100e-3
   * - ``Seed``
     - int
     - 1

The image below illustrates the flowgraph with the steps taken so far:

.. image:: /imgs/bpsk_wav/bpsk_wav_1.png

BPSK demodulation with a wav file
---------------------------------

Now that we've build the modulator, the next step is to place the demodulator building blocks.

To see the constellation received, start by adding the **QT GUI Constellation Sink** block:

.. list-table::
   :widths: 15 15 60
   :header-rows: 1

   * - Field
     - Type
     - Details
   * - ``Type``
     - enum
     - Complex
   * - ``Name``
     - string
     - "__"
   * - ``Number of Points``
     - int
     - 1024
   * - ``Grid``
     - enum
     - Yes
   * - ``Autoscale``
     - enum
     - No
   * - ``Y min``
     - float
     - -2
   * - ``Y max``
     - float
     - 2
   * - ``X min``
     - float
     - -2
   * - ``X max``
     - float
     - -2
   * - ``Number of Inputs``
     - int
     - 1
   * - ``Update Period``
     - float
     - 0.1
   * - ``GUI Hint``
     - gui_hint
     - *blank*


Now add the **Constellation Rect. Object** (this will be necessary for decoding the symbols):

.. list-table::
   :widths: 15 15 60
   :header-rows: 1

   * - Field
     - Type
     - Details
   * - ``Id``
     - id
     - variable_constellation_object
   * - ``Symbol Map``
     - int_vector
     - [0, 1]
   * - ``Constellation Points``
     - complex_vector
     - [-1, 1]
   * - ``Rotational Symmetry``
     - int
     - 4
   * - ``Real Sectors``
     - int
     - 2
   * - ``Imaginary Sectors``
     - int
     - 2
   * - ``Width Real Sectors``
     - int
     - 1
   * - ``Width Imaginary Sectors``
     - int
     - 1
   * - ``Soft bits precision``
     - int
     - 8
   * - ``Soft Decisions LUT``
     - raw
     - None

Place a **Constellation Decoder** block:

.. list-table::
   :widths: 15 15 60
   :header-rows: 1

   * - Field
     - Type
     - Details
   * - ``Constellation Object``
     - raw
     - variable_constellation_object

To map the received points with the respective symbols, use the **Map** block:

.. list-table::
   :widths: 15 15 60
   :header-rows: 1

   * - Field
     - Type
     - Details
   * - ``Map``
     - int_vector
     - [0, 1]

Finally, place the **Repack Bits** block to convert 1 bit to 8 bits per output byte, convert the bytes with **Char To Float** block and play the audio through **Audio Sink** block.

**Repack Bits**:

.. list-table::
   :widths: 15 15 60
   :header-rows: 1

   * - Field
     - Type
     - Details
   * - ``Bits per input byte``
     - int
     - 1
   * - ``Bits per output byte``
     - int
     - 8
   * - ``Length Tag Key``
     - string
     - "__"
   * - ``Packet Alignment``
     - enum
     - Input
   * - ``Endianness``
     - int
     - LSB (1)

**Char To Float**:

.. list-table::
   :widths: 15 15 60
   :header-rows: 1

   * - Field
     - Type
     - Details
   * - ``Vec Length``
     - int
     - 1
   * - ``Scale``
     - float
     - 32

**Audio Sink**:

.. list-table::
   :widths: 15 15 60
   :header-rows: 1

   * - Field
     - Type
     - Details
   * - ``Sample Rate``
     - int
     - 44100
   * - ``Device Name``
     - string
     - *blank*
   * - ``OK to Block``
     - enum
     - Yes
   * - ``Num Inputs``
     - int
     - 1

The complete flowgraph should look like this:

.. image:: /imgs/bpsk_wav/bpsk_wav_2.png

And that's it, you should hear the *wav* through BPSK modulation.

Bonus: visualizing the data bits
--------------------------------

To facilitate debugging, you can visualize the data being sent/received.

Add **Variable** block, **UChar To Float** block right after **Repack Bits** in the modulation section, and another **UChar To Float** right after the **Map** block.

Then, use the **QT GUI Time Sink** to visualize the data for comparison.

**Variable**:

.. list-table::
   :widths: 15 15 60
   :header-rows: 1

   * - Field
     - Type
     - Details
   * - ``Id``
     - id
     - samp_rate
   * - ``Value``
     - raw
     - 192e3

**UChar To Float**:

*no parameters*

**QT GUI Time Sink**:

.. list-table::
   :widths: 15 15 60
   :header-rows: 1

   * - Field
     - Type
     - Details
   * - ``Type``
     - enum
     - Float
   * - ``Name``
     - string
     - "__"
   * - ``Y Axis Label``
     - string
     - Amplitude
   * - ``Y Axis Unit``
     - string
     - "Receiver"
   * - ``Number of Points``
     - int
     - 1024
   * - ``Sample Rate``
     - float
     - samp_rate
   * - ``Grid``
     - enum
     - Yes
   * - ``Autoscale``
     - enum
     - No
   * - ``Y min``
     - float
     - 0
   * - ``Y max``
     - float
     - 1
   * - ``Number of Inputs``
     - int
     - 1
   * - ``Update Period``
     - float
     - 0.1
   * - ``Disp. Tags``
     - enum
     - Yes
   * - ``GUI Hint``
     - gui_hint
     - *blank*

The flowgraph with added visualizing tools should be like the image below:

.. image:: /imgs/bpsk_wav/bpsk_wav_3.png
