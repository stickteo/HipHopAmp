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

#### 24V Boost
At first I simply try using the MT3608 to output 24V directly. There's always some weird noise even with a bench supply. I assume the regulator doesn't perform well when there's not much load. Seemingly the MT3608 picks up noise from its input power pin. It's also sensitive to output capacitance as well.

I was trying a range of output capacitors to try to smooth the ripples away... Generally, more output capacitance makes the ripple have a lower frequency while less capacitance (nothing) increases the frequency. Presumably it depends on the control circuit inside the IC. I would guess it's a comparator with some sort of hysteresis.

I used an earbud to check the noise with no input. (Earbuds are more sensistive to noise compared to "audiophile" headphones.) It was tolerable with a bench supply feeding the MT308 to boost to 24V. (12V from the bench supply acts as ground.) When switching to an off the shelf 12V AC-DC adapter the noise got way worse.

#### 5V to 12V and 24V Boost
Using two MT3608 modules to get 12V and 24V from a single 5V supply doesn't work.

The 5V goes to the input of both modules. With no load, 12V and 24V outputs. However, when I connect earbuds on the output, the 12V output would slowly rise to 24V minus a diode drop.

My best guess is that the MT3608 can't sink current. Presumably if the output sees a higher voltage than what is set, then the output transistor will not turn on. I can only guess the diode drop comes from the output of the opamp. (Since the 4558 opamp is not rail-to-rail.) Thus the output pours current into the 12V line and into the capacitors. Perhaps the slow rise is due to small currents flowing through the headphone. Thus the MT3608 can't regulate any voltages higher than its set point.

There needs to be further testing. Perhaps just using a bench supply at 24V and setting the MT3608 to output 12V using 5V input. Then simply connecting a resistor from 24V to the 12V output. Overall though, the MT3608 is not a good application for generating intermediate voltages.

#### Using LM317 to generate 24V
As a last ditch effort, I try using the LM317 to create a 24V line while using the MT3608 set at 27V. It works okay with a bench supply providing 12V. However, using an AC adapter to provide 12V results in a lot of noise.

Opamps do reject supply noise. However, noise on ground would render it worthless since the opamp only sees 0V and 24V on its negative and positive supply pins. Thus the noise would appear on the inputs. Of course, the 12V input should be regulated as well... Perhaps with another LM317. However, this would make the project overly complicated and too bulky.

## Voltage Divider Circuit
![Split supply circuit.](/images/circuit-split-supply.png)

Of course, the circuit is completely taken from an ESP article.

The circuit is really simple. Two RC circuits in series to create a stable voltage divider. Using that stable voltage, the opamp would buffer the voltage creating a new stable middle voltage.

With the way the schematic is drawn, it's a little misleading that one pushes and the other pulls current. In actuality, using two opamp means doubling the amount of current available to supply to the new rail. Since the 4558 is a dual opamp, might as well as use both.

# LED Music Visualizer
![Hip Hop Led](/images/circuit-hiphopled.png)

A certain special person wanted blinky lights, so I added blinky lights.

I wanted something simple and compact... Thus came brainstorming ideas...

A complex idea is to throw an Arduino Nano at the problem. Get the Nano to read the audio through its analog pins... Do some sort of processing... Then simply output to an LED. However, I don't think it really fits the "theme" of my project.

Thus I search online something like "Audio LED" then came across an Instructables project. It's really simple, simply let an audio signal input into an NPN BJT to control an LED. Very "basic" stuff but some details have to be correct.

So, I just start breadboarding the circuit.

## Instructables BJT LED Driver
![CozzBP Audio Reactive LED Circuit](/images/circuit-instructables-cozzbp.png)

It's really straight forward... But a little too simple for comfort. It definitely works and the BJT will go into saturation. Although the base is commonly conceptualized as a switch with a small current, a more accurate view is a diode instead. The base and the emmitter is literally a diode junction. Without a base resistor, large amounts of current will flow from the base to the emmiter whenever a voltage higher than 0.7V is applied.

By tapping the input with a bench supply, I would see several hundreds of milliamps being drawn. I suppose as long as the BJT doesn't get too hot, it would release the magic smoke... But yeah, the things you only "really" learn by purely building the circuit and not from reading a textbook.

From there, I added a base resistor and tested with audio... It seemed a bit lacking and not bright enough.

## Voltage Offset
In my circuit, R5 and R6 form a voltage divider to set the offset to Q2. This offset should be just below a diode drop. With those values, around 0.5V. If it's too high, the LED will just stay on. Too low and the LED will blink too briefly and not as bright.

A bad point about this is that it depends a lot on the 12V supply. ACDC adapters can have wide output... Sometimes around 13V.

Perhaps to make it more supply agnostic I would use a diode with a resistor divider to create the offset instead.

## Voltage Gain Amplifier
I wanted the LEDs to blink more brightly, thus I implement a simple common-emmitter amplifier with emitter degeneration. 

The gain is set by R3 and R4. I tried having max gain by ommitting R4... However, the effect becomes less subtle.

To calculate R1, R2, R3, and R4:
- V_C is VCC/2 which is 6V
- Desired gain, G, is 20
- Desired quiescent current, I_C, is 1~2mA
- R3 = VCC/2/I_C = 6V / 2mA = ~3kOhm
- R4 = R3/G = 3kOhm / 20 = ~150Ohm
- V_E = R4*I_C = 150Ohm * 2mA = ~0.3V
- V_B = V_E + 0.6V = 0.3V + 0.6V = ~0.9V
- I_B = I_C/10 = 2mA / 10 = 0.2mA
- R2 = V_B/I_B = 0.9V / 0.2mA = ~4.5kOhm
- R1 = (VCC-V_B)/I_B = 11.1V / 0.2mA = ~55.5kOhm

I try to choose convenient values for the actual circuit. Resistances can be slighly lowered to a more convenient value.

# Resources
- ESP Portable Amplifier: https://sound-au.com/project109.htm
- Burr-Brown's AB-051 application note: https://sound-au.com/sboa031.pdf
- ESP Simple DC Adapter Power Supply: https://sound-au.com/project43.htm
- ESP Obtaining +/- Supplies From A Single Power Supply: https://sound-au.com/project192.htm
- CozzBP Audio Reactive LED Circuit: https://www.instructables.com/Creating-An-Audio-Reactive-LED-Circuit/
