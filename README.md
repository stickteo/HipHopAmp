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

# Circuit Design
I was inspired by ESP's Portable Amplifier article. I really like the simplicity of using opamps to drive headphones directly. I did build the crossfeed circuit... However, I just prefer not having crossfeed. Thus the final circuit simply involves volume adjustment, using a dual opamp to buffer the input then two dual opamps to drive the output.

## Main Circuit
![Main Circuit](/images/circuit-main.png)

The circuit is quite simple. The first opamp buffers and amplifies the input by 3 times. The second and third provides the necessary current to output to the headphone.

I didn't show the potentiometer in the circuit. However, R1 and R2 work together with the potentiometer to provide a more logarithmic volume adjustment. Without the resistors, the potentiometer would adjust the volume linearly. Having the resitors in parallel means having more range for quieter volumes.

I kept C1 and C2 so it can block any potential DC. I'm not entirely sure the purpose of R11 and R12.

R3, R5, R4, and R6 sets the overall gain. Without gain, the amplier would only work as a current buffer.

The arrangement of U2 and U3 is very interesting. U2 takes the feedback from the output while U3 acts as a buffer for the output from U3. To have more current, a potential U4 can also act as another buffer like U3.

Aside from the potential to add another buffer, the arrangement makes routing easier as well. This means the opamps can be lined up in a single line while the left and right channels can run on each side. This also means the power rails can be kept in a single line in the center.

## Trying to get a negative rail.
I was again inspired by ESP's article explaining how to use a boost converter to generate 24V and using 12V as ground.

I tried all sorts of configurations:
- Using the MC34063A to create -12V.
- Using 5V to generate 12V and 24V using two MT3608 boost converters.
- Using 12V to generate 24V using one MT3608 boost converter.
- Using LM317 to make it less noisy.

Only the third configuration worked. Though quite disappointingly noisy.

### MC34063A
So I found this IC inside a USB charger that plugs into a car's "cigarette lighter". Essentially the adapter takes in 12V and spits out 5V.

The MC34063A is a control IC that can step-up, step-down, and invert voltages.

I try to build a circuit with it but I ended up blowing up the chip. I think I didn't have the right inductor to properly use it. Building a switching supply can have all sorts of pitfalls. Though it's something I have to try again. Well, it was a bit too complicated and I just wanted a headphone amplifier.

### MT3608
Since trying to build a switching regulator failed, I turned my attention to cheap Chinese boost converters. The MT3608 seemed fairly small and compact.

At first I simply try using the MT3608 to output 24V directly. There's always some weird noise even with a bench supply. I assume the regulator doesn't perform well when there's not much load. Seemingly the MT3608 picks up noise from its input power pin. It's also sensitive to output capacitance as well. (Trying a range of output capacitors to try to smooth the ripples away...)

### 

# Resources
- ESP Portable Amplifier: https://sound-au.com/project109.htm
- Burr-Brown's AB-051 application note: https://sound-au.com/sboa031.pdf
- ESP Simple DC Adapter Power Supply: https://sound-au.com/project43.htm
- ESP Obtaining +/- Supplies From A Single Power Supply: https://sound-au.com/project192.htm
