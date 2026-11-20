# HDSoCv2_ROI_WLC_capture
ROI mode and WLC mode with external trigger on HD-SoCv2

HDSoCv2 WLC ROI capture

ROI-mode capture with an external trigger on the HDSoCv2 eval board (hdsocv2_evalr2), adapted from Nalu's hdsocv1_evalr2_wlc_roi_capture.ipynb.

Needs naludaq 0.36.1 on a Python 3.11 kernel. An older naludaq under Python 3.9 breaks during pedestal generation. Set the board and receiver addresses in the init cell to match your network.

Signal generator

Set by hand on the front panel. The notebook doesn't control the generator.

CH1 → signal input: sine, 7.8125 MHz, 0.3 Vpp, 0 V offset, 50 Ω load
CH2 → external trigger: pulse, 1 MHz, 2.5 V high / 0 V low, 50 ns wide, 50 Ω load. Burst on: 3 cycles every 1 s, internal trigger source.

Turn both outputs off while generating pedestals and on for readout.

Changes from the Nalu version
Board model changed to hdsocv2_evalr2.
startup_board runs twice. With a single call, the chip returned no events.
naluinstruments (Nalu-internal) is replaced by a do-nothing ManualOutput stand-in.
Trigger cell updated for v2:
The threshold is on channel 2 at 37 counts (was channel 8 at 100). In ROI mode the threshold has to sit inside the signal's swing, or nothing gets read out. 37 comes from a scaler scan with this sine and DAC 1804. Re-scan if either changes.
Four trigger reference groups instead of two.
The v1 tsel/tsgn register writes are replaced by write_triggers() and BOARD.trigger.edges.
The plot cell finds the channels that have data instead of assuming channel 8.
New cells: one to plot all events, and one to save each event to figures/<acquisition>/event_NNNN.png.
