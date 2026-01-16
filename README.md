# Modification and Implementeation of the Arduino Uno PedalShield 

<img src="images/Pedal.jpg" alt="Diagram" width="400">

Figure 1: Working prototype in action.

# Motivation & Overview

As a guitarist and recent graduate in electronics engineering I am fascinated by audio effects and more specifically guitar pedals. In fact, It was one of my main motivations to study electronics engineering in the first place, to be able to make my own pedals and experiment with them. Throughout my university years, some of that passion faded away since the educations is not all about designing circuits and ended up putting this hobby aside. Now finally the spark of curiosity lighted again and I felt more motivated than explore the world of audio effects both in hardware and in software.

I came across the [pedalSHIELD UNO Arduino Guitar Pedal](https://www.electrosmash.com/pedalshield-uno "pedalSHIELD UNO Arduino Guitar Pedal.") from ElectroSmash and I felt determined to make it and learn as much as possible from the process. To do so I followed the following procedure:
1. Understanding conceptually the main components and its functions
2. Analyzing the main circuits mathematically (bode plots etc.)
3. Making a simulation (with LT Spice) to analyze and validate
4. Building the circuit on the breadboard and analyzing it with the oscilloscope 
5. Designing the PCB
6. Final test

In this file, however, I will be concise in what modifications I made to the original design and lastly I will share my experience about implementing it. 

I will upload a file with the analysis soon to this repo. In the meantime here is a list of what I think the requirements should be:
## Main requirements summarized:
- [[Input Stage]]
	- Since the sampling frequency is set to fs​≈31.25kHz, we need to filter frequencies above 15.6 kHz to avoid aliasing. And the guitar frequency can range from 82 to 5kHz. So the required cutoff frequency of the low pass filter can be narrowed down to 5 kHz.
	- The guitar signal is boosted from around one 1dB-21dB in the original design.
	
- Output Stage
	- The output signal is produced by a PWM of frequency 33kHz. That signal should be heavily attenuated. To avoid affecting the guitar frequency the cutoff frequency the low pass filter cutoff frequency is set to 5kHz.


## Key Modification to the Original Design

- Fix one issue with the original design:
	1) ~={green}Changed the capacitors $C_7$, $C_8$, $C_9$ to 2.2 nF=~: In the original design, the cutoff frequency of the output low pass filter is not the desired 5 kHz. Therefore I change the $RC = \frac{2 \pi}{1675}\approx 0.00375$. So If I use Resistor equal to 4.7 k ohms then C must be around $2.2 n F$. For a slightly lower cutoff frequency C can be higher.

## Implementation notes

<img src="images/PCB_PedalShield.png" alt="PCB" width="400">
figure 2: The PCB I designed in KiCad. Note that I fliped the name of the Input and Output Jacks; it should be the other way around.

I designed the PCB in KiCad and since it is the first prototype I omitted some non-essential aspects of the design to make it simpler for myself:
- The foot-switch
- The two buttons that are used to control some parameters of the effects.

For the jacks, I had to make a custom footprint out of the measurements that where given in their Amazon page. Luckily it fitted perfectly! 
(show picture)

I didn't have a capacitor of 270pF. So I chose a 330pf that a had in stock. This lowers the cutoff frequency of the Low-pass filter slightly (with R3 and C2) --> $f_c=\frac{1}{2 \pi C_4 R_5}\approx 4.8229 KHz$

After soldering all components to the board I realized an issue, the potentiometer did not fit!. Due to that, and since it is just a prototype, I decided to solder some cables and hook the potentiometer onto a breadboard, as you can see in figure 1.

Finally, I was excited to test it and realized to my surprise that it worked without issues. Then I played around with some effects that ElectroSmash provides in his repository https://github.com/ElectroSmash/pedalshield-uno. I also realized why he made a new version with the Arduino Due, since the Arduino Uno really struggles with a simple delay effect due to its low memory.


