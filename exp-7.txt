clc; close all;

% Time and frequency setup
t = -5:0.01:5;         % Continuous time
f = 2; w = 2 * pi * f; % Frequency and angular frequency
ts = -5:1/(w * 250 / pi):5;  % Sampled time based on oversampling ratio

% Signal definition
y = @(t) sin(w * t);   % Original sinusoidal signal
q = sign(y(ts));       % SDQ: quantized signal

% Reconstruct the signal
z = sum(q' .* sinc(w * (t - ts')), 1);  % Sum of sinc functions for reconstruction
z = z * (max(y(t)) / max(z));           % Scaling to match the original signal

% Plotting results
subplot(311), plot(t, y(t), 'linewidth', 2), title('Original signal'), xlabel('Time'), ylabel('Amplitude')
subplot(312), plot(ts, q), title('SDQ signal'), xlabel('Time'), ylabel('Amplitude')
subplot(313), plot(t, z, 'linewidth', 2), title('Reconstructed signal'), xlabel('Time'), ylabel('Amplitude')

figure, plot(t, y(t), 'linewidth', 2), hold on, plot(t, z, 'linewidth', 2), title('Original vs Reconstructed')

figure, plot(abs(z - y(t)), 'linewidth', 0.5), title('Error')

% Spectrum analysis
figure
subplot(311), plot(abs(fftshift(fft(y(t)))) / length(t)), title('Spectrum of Original')
subplot(312), plot(abs(fftshift(fft(q))) / length(ts)), title('Spectrum of SDQ')
subplot(313), plot(abs(fftshift(fft(z))) / length(t)), title('Spectrum of Reconstructed')
