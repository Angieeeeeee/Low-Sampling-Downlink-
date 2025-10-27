# Designing Low-Correlation OFDM Symbols For WiFi Payload 
_Enabling Payload Recovery at Sub-Nyquist Receiver_
---
Problem: Most IoT devices are designed to be low-power and thus have slow oscillators. With a very slow signal sampling rate, how can these low-power IoT devices receive information from high-frequency WiFi packets?
Goal: To design symbols in the WiFi payload that maintain low cross correlation even with distortion due to low-sampling, CFO, and channel distortion, while remaining within WiFi packet standards. 
## My Approach 
If two frequency patterns 𝑋_𝐴 [𝑘]and 𝑋_𝐵 [𝑘]are orthogonal, the corresponding time-domain waveforms 𝑥_𝐴 [𝑛]and 𝑥_𝐵 [𝑛]will have low cross correlation.
Make A and B use disjoint bins (A uses even data bins, B uses odd data bins), then for every k, at least one of 𝑋_𝐴 [𝑘]or 𝑋_𝐵 [𝑘]is zero.
The only frequencies in common between symbols A and B are the pilot bins used for channel estimation. 
### Code 
The signal was originally made with the wlan MATLAB functions to make sure the STF, LTF, and preamble are set up correctly.
The payload is filled with 64 random symbols and overwritten with a pattern sequence of 64 As and Bs (avoiding the scrambling and interleaving that the MATLAB function applies).
Symbols A and B are made with even/odd frequency bins and then turned into time domain signals using ifft(), adding Cyclic Prefix to start to stay in accordance with OFDM requirements.

Rx side simulation:
From the ideas of autocorrelation we looked at in previous research papers, I applied comparison windows with time lag = 1 symbol.
Payload designed to always start with symbol A, I check if the next symbol has a high correlation with the previous symbol (A) or low correlation (B).

## Outcome [SO FAR]
In an ideal setting, the method was effective in recovering the payload symbol sequence even after downsampling by 4 and 8 (Rx = 5MHz and 2.5MHz) with a 0% error rate.
When at full sampling rate, the correlation was either 0 or 1. As the sampling rate decreases, the correlation value becomes less polarized. 

<img width="441" height="521" alt="image" src="https://github.com/user-attachments/assets/6be80948-a176-4fe4-b76e-95fd967f441a" />
