# HipHopAmp
Opamp based headphone amplifier build project.

# Introduction
This project is divided into five parts:
- Main Amplifier circuit
- Voltage Divider circuit
- LED Music Visualizer
- Circuit Routing & Soldering
- Case Design and 3D Print

I wanted to make a headphone amplifier. The choice to use opamps was due to its simplicity and not needing to use any extra heatsinks.

# Amplifier Circuit
I was inspired by ESP's Portable Amplifier article. I really like the simplicity of using opamps to drive headphones directly. I did build the crossfeed circuit... However, I just prefer not having crossfeed. Thus the final circuit simply involves volume adjustment, using a dual opamp to buffer the input then two dual opamps to drive the output.

# Resources
- ESP Portable Amplifier: https://sound-au.com/project109.htm
- Burr-Brown's AB-051 application note: https://sound-au.com/sboa031.pdf
- ESP Simple DC Adapter Power Supply: https://sound-au.com/project43.htm
