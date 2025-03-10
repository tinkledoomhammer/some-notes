
# Shockley diode equation

$$I=I_S(e^{\frac{V_d}{n V_T}}-1)$$

$V_T= \frac{k T}{q}\approxeq24-27mV$
* $k$: Boltzman's constant
* $q$: the fundamental charge of an electron

$n\approxeq 1-2$
* $n$ is how 'ideal' the diode is. 1 means ideal. Values are rarely greater than 2.

When the exponential term is high, $I \approxeq I_S e^{\frac{V_d}{n V_T} }$.

The parameters  $I$ and $n$ can be measured. $I_S$ and $V_d$ can then be calculated.
$I_n=I_s[\exp(\frac{V_n}{nV_T})-1]$
Using two samples:
$I_1=I_s[\exp(\frac{V_1}{n V_T})-1]$;  $I_2=I_s[\exp(\frac{V_2}{n V_T})-1]$.
With $\frac{I_2}{I_s}=\frac{I_1\frac{I_2}{I_1}}{I_s}$,
$$
\exp(\frac{V_2}{nV_T})-1 = \frac{I_2}{I_1}[\exp(\frac{V_1}{n V_T})-1]
$$$$
\exp(\frac{V_2}{e V_T n})= \frac{I_2}{I_1}\exp(\frac{V_1}{e  V_T n})
$$$$
\frac{V_2}{e V_T n} = \frac{V_1}{e V_T n}+ln(\frac{I_2}{I_1})
$$$$
\frac{V_2-V_1}{e V_T n}=\ln(\frac{I_2}{I_1})
$$$$
n = \frac{V_2-V_1}{e V_T ln(I_2/I_1)}$$
and $$
I_S=\frac{I_n}{\exp(\frac{V_n}{e V_t n})}$$



# Timing and such
1. Interrupt driven frame counter. Probably ~500hz, i.e. timer0 on arduino uno/nano
2. Main loop
	3. Debounce inputs
	4. update and transmit new colors when appropriate

## Color tracking
1. Pallet entries are stored at a lower color depth
2. Actual transmission:
	1. 24bpp, 1.2us/bit, 800kbps
	2. 30k pixels/sec, 30us/pixel
	3. 50us reset/end transmission
	4. @250hz, 7k pixels per cycle
3. Current pixel values are stored as 24-bit numbers
	1. 3 bytes/pixel

> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTQxMzY4MTExNl19
-->