# GENERATION-AND-DETECTION-OF-FM
## AIM:
To write a program for Frequency Modulation and Demodulation using SCILAB and to observe and measure the frequency deviation and the modulation index of FM.

## EQUIPMENTS REQUIRED

•	Computer with i3 Processor

•	SCI LAB

## THEORY
  Frequency modulation is a type of modulation in which the frequency of the high frequency (carrier) is varied in accordance with the instantaneous value of the modulating signal.
  
  #### FREQUENCY DEVIATION Δf  and MODULATION INDEX m f :
  
  The frequency deviation Δf represents the maximum shift between the  modulated signal
  frequency, over and under the frequency of the carrier.
  
  We define modulation index m f the ratio between Δf and the modulating frequency
  m= Δf / fm


#### FREQUENCY MODULATION GENERATION:
  The circuits used to generate a frequency modulation must vary the frequency of a high frequency signal (carrier) as function of the amplitude of a low frequency signal (modulating signal). In practice there are two main methods used to generate FM.

## ALGORITHM
  1.	Define Parameters:
     
      •	Fs: Sampling frequency.
      •	T: Duration of the signal.
      •	Fc: Carrier frequency.
      •	Fm: Frequency of the modulating signal.
      •	Beta: Modulation index, which controls the extent of frequency deviation.
  2.	Generate Signals:
     
      •	modulating_signal: Sinusoidal signal used for modulation.
      •	carrier_signal: The high-frequency carrier signal.
      •	modulated_signal: FM modulated signal calculated by varying the carrier frequency according to the modulating signal.
      
  3.	FM Modulation:
     
      •	Modulated_signal is obtained by modulating the carrier signal with the modulating signal.
 
  4.	FM Demodulation:
     
      •	Differentiation: Computes the derivative of the modulated signal to extract frequency variations.
      •	Envelope Detection: Takes the absolute value to retrieve the envelope of the signal.
      •	Low-pass Filtering: Applies a Butterworth low-pass filter to smooth the envelope and recover the original modulating signal.
      
  5.	Visualization:
      
      •	Plots the modulating signal, carrier signal, FM modulated signal, and demodulated signal for analysis.


## PROCEDURE

    •	Refer Algorithms and write code for the experiment.
    •	Open SCILAB in System
    •	Type your code in New Editor
    •	Save the file
    •	Execute the code
    •	If any Error, correct it in code and execute again
    Verify the generated waveform using Tabulation and Model Waveform

## MODEL GRAPH:
<img width="512" height="365" alt="image" src="https://github.com/user-attachments/assets/dfe6bc64-2b6f-4afa-ae79-95391859ab04" />

## PROGRAM
// =================================================================
// AIM: AM Generation, Detection, and Modulation Index Calculation
// =================================================================

// 1. Define Parameters (Provided by user)
Am = 14.7;      // Modulating Signal Amplitude
Ac = 29.4;      // Carrier Signal Amplitude
fm = 1453;      // Modulating Signal Frequency (Hz)
fc = 14530;     // Carrier Signal Frequency (Hz)
Fs = 145300;    // Sampling Frequency (Hz)

// Calculated Parameters
ma = Am / Ac;   // Modulation Index (0.5)
T = 3 / fm;     // Duration of the signal (Show 3 cycles of message signal)

disp("--- AM Parameters ---");
disp("Theoretical Modulation Index (ma = Am/Ac) = " + string(ma));
disp("-----------------------");


// 2. Create a time vector
t = 0:1/Fs:T;
N = length(t); // Number of samples


// 3. Create Modulating Signal
m_t = Am * sin(2 * %pi * fm * t);

// 4. Create Carrier Signal
c_t = Ac * sin(2 * %pi * fc * t);

// 5. Perform Amplitude Modulation
// s(t) = Ac * (1 + ma * sin(2*pi*fm*t)) * sin(2*pi*fc*t)
// CRITICAL: Use .* for element-wise multiplication
s_t = Ac * (1 + ma * sin(2 * %pi * fm * t)) .* sin(2 * %pi * fc * t);


// 6. Plot the Signals
figure(1);
clf(); // Clear current figure

subplot(3, 1, 1);
plot(t, m_t);
title("1. Modulating Signal (fm=" + string(fm) + " Hz)");
xlabel("Time (s)");
ylabel("Amplitude (V)");
xgrid();

subplot(3, 1, 2);
plot(t, c_t);
title("2. Carrier Signal (fc=" + string(fc) + " Hz)");
xlabel("Time (s)");
ylabel("Amplitude (V)");
xgrid();

subplot(3, 1, 3);
plot(t, s_t);
title("3. Amplitude Modulated Signal (ma=" + string(ma) + ")");
xlabel("Time (s)");
ylabel("Amplitude (V)");
xgrid();


// -----------------------------------------------------------------
// CALCULATION OF PRACTICAL MODULATION INDEX
// -----------------------------------------------------------------

// Theoretical Envelope Max/Min
E_max_env = Ac * (1 + ma);
E_min_env = Ac * (1 - ma);

// Practical Calculation using the formula: ma = (Emax - Emin) / (Emax + Emin)
ma_practical = (E_max_env - E_min_env) / (E_max_env + E_min_env);

disp("--- Modulation Index Calculation ---");
disp("E_max (Theoretical Envelope) = " + string(E_max_env) + " V");
disp("E_min (Theoretical Envelope) = " + string(E_min_env) + " V");
disp("Practical Modulation Index (ma_practical) = " + string(ma_practical));


// -----------------------------------------------------------------
// 7. Demodulate the AM Signal (Envelope Detection using Moving Average)
// -----------------------------------------------------------------

// Step 1: Rectification (Keep only the positive envelope)
rectified_s_t = max(0, s_t);

// Step 2: Low-Pass Filtering (Moving Average Filter)
// Window size M must be > 1/fc and < 1/fm.
M_samples = round(Fs / (fm * 5)); // Use 1/5th of the message period
if M_samples < 2 then M_samples = 2; end

demodulated_s_t = zeros(1, N);
// Loop for Moving Average (LPF approximation)
for i = M_samples:N
    demodulated_s_t(i) = mean(rectified_s_t(i - M_samples + 1 : i));
end

// DC Block (Subtract the carrier DC bias, which should be close to Ac)
DC_bias = Ac;
demodulated_s_t = demodulated_s_t - DC_bias;

// Scale and adjust for comparison
scaling_factor = Am / max(abs(demodulated_s_t));
demodulated_s_t = demodulated_s_t * scaling_factor;


// 8. Plot the Demodulated Signal
figure(2);
clf();

// Plot the original message signal and the demodulated signal for comparison
plot(t, m_t, 'b--', 'linewidth', 2); // Original message signal (dashed blue)
plot(t, demodulated_s_t, 'r-'); // Demodulated signal (solid red)
title("AM Demodulation: Original vs. Recovered Message");
xlabel("Time (s)");
ylabel("Amplitude (V)");
legend(["Original Message", "Demodulated Signal"], 4);
xgrid();


## TABULATION
![WhatsApp Image 2025-11-19 at 19 53 23_c8f866f1](https://github.com/user-attachments/assets/06fcb801-5860-4c93-af35-cf4d090af636)

## CALCULATION
![WhatsApp Image 2025-11-19 at 19 54 53_f29f79ba](https://github.com/user-attachments/assets/650d36f0-69c1-4de3-8c8a-ee0fee60997b)

## OUTPUT
![WhatsApp Image 2025-11-19 at 19 53 46_29a69fdf](https://github.com/user-attachments/assets/b8e98c34-05d3-43c4-95c6-47544bd7b9b6)

## RESULT
Thus the frequency modulation and demodulation is successfully done and the output is experimentally verified


