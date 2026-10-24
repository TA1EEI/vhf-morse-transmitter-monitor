---
title: 'VHF Morse Code Transmission via Display Pixel-Clock Harmonics and EMI Leakage'
tags:
  - JavaScript
  - HTML5 Canvas
  - electromagnetic side-channel
  - TEMPEST
  - SDR
  - amateur radio
  - VHF
authors:
  - name: Efe Işık
    orcid: 0009-0001-1628-2183
    affiliation: 1
affiliations:
 - name: Independent Researcher (Callsign TA1EEI), Türkiye
   index: 1
date: 24 October 2026
bibliography: paper.bib
---

# Summary

Modern video pipelines are high-speed digital systems that generate unintentional electromagnetic radiation (EMI) during normal operation[cite: 1]. The **VHF Monitor RF Transmitter** is a browser-based experimental and educational tool that converts a standard computer display into a short-range Continuous Wave (CW) and Amplitude Modulated (AM) Morse code transmitter in the VHF spectrum without requiring external radio hardware, SDRs, or modems[cite: 1]. By controlling pixel output via an HTML5 Canvas rendering engine, the software manipulates the display's video processing pipeline and transmission interfaces (HDMI/DisplayPort) to radiate modulated electromagnetic signals through physical side-channel emissions[cite: 1].

# Statement of Need

Electromagnetic side-channel research (commonly known as TEMPEST) demonstrates how unintentional radio-frequency emanations from computing equipment can be exploited for data exfiltration or covert communication [@kuhn1998soft; @guri2014airhopper]. However, reproducing these phenomena in educational or laboratory settings often requires specialized hardware, custom FPGA designs, or platform-specific kernel modifications. 

The **VHF Monitor RF Transmitter** addresses this limitation by providing a zero-installation, cross-platform web application capable of generating reproducible RF harmonics directly from a standard web browser[cite: 1]. Highlighted by the UK National Cyber Security Centre (NCSC), Hackaday, and RTL-SDR.com, the tool serves security researchers, educators, and amateur radio operators investigating physical-layer security and video EMI leakage. It also includes automatic localization across six languages (English, Turkish, French, German, Spanish, and Chinese), dynamic refresh-rate probing via high-resolution timers, and built-in photosensitive epilepsy safety warnings[cite: 1].

# Physics and Engineering of Display EMI

To display a stable image, a graphics card pushes video data frame-by-frame governed by the **Pixel Clock Frequency** ($f_{\text{pixel}}$), calculated from total frame dimensions (active pixels plus horizontal and vertical blanking intervals) and the refresh rate ($f_{\text{refresh}}$):

$$f_{\text{pixel}} = H_{\text{total}} \times V_{\text{total}} \times f_{\text{refresh}}$$

For a standard 1080p display at 60 Hz following the CEA-861 timing standard ($H_{\text{total}} = 2200$, $V_{\text{total}} = 1125$):

$$f_{\text{pixel}} = 2200 \times 1125 \times 60 = 148.500\text{ MHz}$$

When the software renders an alternating $1 \times 1$ black (`#000000`) and white (`#FFFFFF`) pixel pattern, every adjacent clock cycle forces the display interface serializer (TMDS/FRL in HDMI or Main Link Lanes in DisplayPort) to toggle its voltage state rapidly[cite: 1]. This induces a carrier wave at $f_{\text{pixel}}$ (and its higher harmonics $2 f_{\text{pixel}}$, $3 f_{\text{pixel}}$). By switching between the high-contrast alternating pattern (Carrier ON) and a solid black canvas (Carrier OFF) according to standardized Morse code timings, nearby VHF receivers or SDRs can demodulate audible CW/AM tones[cite: 1].

| Resolution | Refresh Rate (Hz) | Total Timings ($H_{\text{total}} \times V_{\text{total}}$) | Fundamental Target Frequency |
| :--- | :--- | :--- | :--- |
| **1920 x 1080** | 60 Hz | $2200 \times 1125$ | **148.500 MHz** (VHF Amateur 2m Band) |
| **1920 x 1080** | 144 Hz | $2200 \times 1125$ | **356.400 MHz** (UHF Band) |
| **2560 x 1440** | 60 Hz | $2720 \times 1481$ | **241.500 MHz** (VHF High) |
| **1280 x 720** | 60 Hz | $1650 \times 750$ | **74.250 MHz** (VHF Low / 4m Band) |

# References
