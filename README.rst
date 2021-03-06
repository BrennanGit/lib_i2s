.. |I2S| replace:: I\ :sup:`2`\ S

I2S/TDM Library
===============

Summary
-------

A software library that allows you to control an |I2S| or TDM (time
division multiplexed) bus via xCORE ports. |I2S| and TDM are digital
data streaming interface particularly appropriate for transmission of
audio data. The components in the libary
are controlled via C using the XMOS multicore extensions (xC) and
can either act as |I2S| master, TDM master or |I2S| slave.

Features
........

 * |I2S| master (sample and frame-based), TDM master and |I2S| slave modes.
 * Handles multiple input and output data lines.
 * Support for standard |I2S|, left justified or right justified
   data modes for |I2S|.
 * Support for multiple formats of TDM synchronization signal.
 * Sample rate support up to 192KHz.
 * Up to 32 channels in/32 channels out (depending on sample rate)

Resource Usage
..............

.. resusage::

  * - configuration: |I2S| Master
    - globals:   out buffered port:32 p_dout[2] = {XS1_PORT_1D, XS1_PORT_1E};
                 in buffered port:32 p_din[2]  = {XS1_PORT_1I, XS1_PORT_1K};
                 port p_mclk  = XS1_PORT_1M;
                 out buffered port:32 p_bclk  = XS1_PORT_1A;
                 out buffered port:32 p_lrclk = XS1_PORT_1C;
                 clock mclk = XS1_CLKBLK_1;
                 clock bclk = XS1_CLKBLK_2;
    - locals: interface i2s_callback_if i;
    - fn: i2s_master(i, p_dout, 2, p_din, 2, p_bclk, p_lrclk, bclk, mclk);
    - pins: 3 + data lines
    - ports: 3 x (1-bit) + data lines
    - cores: 1
    - target: XCORE-200-EXPLORER
  * - configuration: |I2S| Master (frame-based)
    - globals:   out buffered port:32 p_dout[2] = {XS1_PORT_1D, XS1_PORT_1E};
                 in buffered port:32 p_din[2]  = {XS1_PORT_1I, XS1_PORT_1K};
                 port p_mclk  = XS1_PORT_1M;
                 out port p_bclk  = XS1_PORT_1A;
                 out buffered port:32 p_lrclk = XS1_PORT_1C;
                 clock mclk = XS1_CLKBLK_1;
                 clock bclk = XS1_CLKBLK_2;
    - locals: interface i2s_frame_callback_if i;
    - fn: i2s_frame_master(i, p_dout, 2, p_din, 2, p_bclk, p_lrclk, p_mclk, bclk);
    - pins: 3 + data lines
    - ports: 3 x (1-bit) + data lines
    - cores: 1
    - target: XCORE-200-EXPLORER
  * - configuration: |I2S| Slave
    - globals:   out buffered port:32 p_dout[2] = {XS1_PORT_1D, XS1_PORT_1E};
                 in buffered port:32 p_din[2]  = {XS1_PORT_1I, XS1_PORT_1K};
                 port p_mclk  = XS1_PORT_1M;
                 in port p_bclk  = XS1_PORT_1A;
                 in buffered port:32 p_lrclk = XS1_PORT_1C;
                 clock bclk = XS1_CLKBLK_2;
    - locals: interface i2s_callback_if i;
    - fn: i2s_slave(i, p_dout, 2, p_din, 2, p_bclk, p_lrclk, bclk);
    - pins: 2 + data lines
    - ports: 2 x (1-bit) + data lines
    - cores: 1
    - target: XCORE-200-EXPLORER
  * - configuration: TDM Master
    - globals:   out buffered port:32 p_dout[2] = {XS1_PORT_1D, XS1_PORT_1E};
                 in buffered port:32 p_din[2]  = {XS1_PORT_1I, XS1_PORT_1K};
                 out buffered port:32 p_fsync = XS1_PORT_1C;
                 clock bclk = XS1_CLKBLK_2;
    - locals: interface i2s_callback_if i;
    - fn: tdm_master(i, p_fsync, p_dout, 2, p_din, 2, bclk);
    - pins: 2 + data lines
    - ports: 2 x (1-bit) + data lines
    - cores: 1
    - target: XCORE-200-EXPLORER

Software version and dependencies
.................................

.. libdeps::

Related application notes
.........................

The following application notes use this library:

  * AN00162 - Using the |I2S| library
