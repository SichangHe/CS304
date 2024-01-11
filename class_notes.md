# CS304 class note

broader speech processing

- wake-up word detection: binary classification
    - small window → low latency
    - multiple models, threshold & size
- echo cancellation: subtract (complex) echo of sound from speaker
- sound source localization: microphone array, delay

online/streaming mode: continuous conversion

omnidirectional/ cardioid/ hyper cardioid microphone

sound sample rate

- need at lease 2x of highest frequency wanted, else aliasing (Nyquist theorem)
- 44.1kHz for CD
- 16kHz good enough for speech

$\mu$-curve

audio format: use PCM `.wav`

online endpointing format: push to talk, hit to talk, continuous listening

## feature extraction

spectrogram: energy distribution over frequency vs time

raw sound data to spectrogram:

- discrete Fourier transform (DFT): convert from time series to frequency domain
    - symmetric, only first half meaningful
    - magnitude: $0\sim\frac{S_R}{2}$Hz, frequency $\frac{i}{M}S_R$ at point $i$
    - flat noise from jump in sample windowing
        - solution: multiple input by bell-shape windowing function
            and half-overlap windows to avoid losing information
    - before using fast Fourier transform (FFT): zero padding
        - cause fake interpolated detail

preemphasize speech signal: boost high frequency

$$
s_{preemp}(n) = s(n) - \alpha s(n-1)\\
\alpha=0.95
$$
