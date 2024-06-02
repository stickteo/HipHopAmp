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

## Main Circuit
![Main Circuit](/images/circuit-main.png)

The circuit is quite simple. The first opamp buffers and amplifies the input by 3 times. The second and third provides the necessary current to output to the headphone.

I didn't show the potentiometer in the circuit. However, R1 and R2 work together with the potentiometer to provide a more logarithmic volume adjustment. Without the resistors, the potentiometer would adjust the volume linearly. Having the resitors in parallel means having more range for quieter volumes.

I kept C1 and C2 so it can block any potential DC. I'm not entirely sure the purpose of R11 and R12.

R3, R5, R4, and R6 sets the overall gain. Without gain, the amplier would only work as a current buffer.

The arrangement of U2 and U3 is very interesting. U2 takes the feedback from the output while U3 acts as a buffer for the output from U3. To have more current, a potential U4 can also act as another buffer like U3.

Aside from the potential to add another buffer, the arrangement makes routing easier as well. This means the opamps can be lined up in a single line while the left and right channels can run on each side. This also means the power rails can be kept in a single line in the center.



# Resources
- ESP Portable Amplifier: https://sound-au.com/project109.htm
- Burr-Brown's AB-051 application note: https://sound-au.com/sboa031.pdf
- ESP Simple DC Adapter Power Supply: https://sound-au.com/project43.htm
