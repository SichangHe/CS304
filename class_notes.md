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


1. preemphasize speech signal: boost high frequency

    $$
    s_{preemp}(n) = s(n) - \alpha s(n-1)\\
    \alpha=0.95
    $$

1. spectrogram: from time series to frequency domain
    - discrete Fourier transform (DFT)
    - symmetric, only first half + midpoint meaningful (by Nyquist theorem)
    - magnitude: $0\sim\frac{S_R}{2}$Hz, frequency $\frac{i}{M}S_R$ at point $i$
        - $S_R$: sample rate, $M$: number of sample
        - power: magnitude squared
    - flat noise from jump in sample windowing
        - solution: multiple input by bell-shape *windowing function*
            and half-overlap windows to avoid losing information
    - before using fast Fourier transform (FFT): zero padding
        - cause fake interpolated detail
1. auditory perception: from frequency to Bark
    - mimic human ear, which distinguish low frequency better
    - frequency warping with Mel curve
    - filter bank: triangular filter on Mel curve to be multiplied
        - evenly-spaced, half-overlap, 40 enough, 80 good
        - translate back to equal-area triangles on frequency axis
            - not directly on Mel curve for simplicity
1. log Mel spectrum: log of integration of each filter bank
1. Mel cepstrum discrete cosine transform (DCT) compression
    - reduce redundancy from filter bank overlap
    - subtract the mean to remove microphone/noise difference
        - microphone response is speech with convolution
        - convolution -DFT → multiplication -log → summation
