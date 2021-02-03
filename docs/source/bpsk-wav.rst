Creating a BPSK modulation with audio
=====================================

BPSK modulation with a *wav* file
---------------------------------

First, place an *wav* audio file in the project's directory, then add the *Wav File Source* block with the following parameters:

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

Then, use the *Float To Char* block to convert the wav data into char arrays:

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

Now, to convert the raw data to BSPK modulation, use the *Repack Bits* with 8 bits per input byte and 1 bit per output byte, since the BPSK modulation will generate a pair of symbols mapped to (0,1):

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
     - ""
   * - ``Packet Alignment``
     - enum
     - Input
   * - ``Endianness``
     - int
     - LSB (1)

To effectively convert the individual bits to symbols, use the *Chunks to Symbols* block with the following parameters:

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

Finally, add the *Noise Source* block to generate some noise to the modulation:

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
