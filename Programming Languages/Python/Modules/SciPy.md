---
tags:
  - scipy
  - python
  - filter
  - data-processing
---

# Scipy
## Filter Design (-> [scipy-style](https://docs.scipy.org/doc/scipy/reference/signal.html#filter-design), [matlab-style](https://docs.scipy.org/doc/scipy/reference/signal.html#matlab-style-iir-filter-design))
> [!INFO]- References
> [[References#^refbandpassfilter]]

### [Butterworth](https://docs.scipy.org/doc/scipy/reference/generated/scipy.signal.butter.html)
`scipy.signal.butter(N, Wn, btype='low', ...)`
* N
	* The order of the filter
* Wn
	* The critical frequency or frequencies
	* Scalar for lowpass & highpass filters
* btypes
	* {'lowpass', 'highpass', 'bandpass', 'bandstop'}
	* Default: 'lowpass'
* fs
	* Sampling frequency
	* *New in version 1.2.0*

#### Bandpass Filter
```python
# https://scipy-cookbook.readthedocs.io/items/ButterworthBandpass.html
from scipy.signal import butter, lfilter

def butter_bandpass(lowcut, highcut, fs, order=5):
    nyq = 0.5 * fs # Nyquist Frequency
    low = lowcut / nyq
    high = highcut / nyq
    b, a = butter(order, [low, high], btype='band')
    return b, a

def butter_bandpass_filter(data, lowcut, highcut, fs, order=5):
    b, a = butter_bandpass(lowcut, highcut, fs, order=order)
    y = lfilter(b, a, data)
    return y
```

> [!WARNING]+
> SciPy bandpass filters designed with b, a are [unstable](https://stackoverflow.com/questions/21862777/bandpass-butterworth-filter-frequencies-in-scipy) and may result in [erroneous filters](https://stackoverflow.com/questions/41371915/20hz-20000hz-butterworth-filtering-exploding) at **[higher filter orders](https://dsp.stackexchange.com/questions/17235/filtfilt-giving-unexpected-results)**.
> 
> Instead, use sos (second-order sections) output of filter design.

```python
from scipy.signal import butter, sosfilt, sosfreqz

def butter_bandpass(lowcut, highcut, fs, order=5):
        nyq = 0.5 * fs # Nyquist Frequency
        low = lowcut / nyq
        high = highcut / nyq
        # Second-Order Sections
        sos = butter(order, [low, high], analog=False, btype='band', output='sos')
        return sos

def butter_bandpass_filter(data, lowcut, highcut, fs, order=5):
        sos = butter_bandpass(lowcut, highcut, fs, order=order)
        y = sosfilt(sos, data)
        return y
```

#### Lowpass Filter
```python
from scipy.signal import lfilter, butter

def low_butter_filter(x, order, freq=0.05):
	b, a = butter(order, freq, btype='low')
	return lfilter(b, a, x)
```

## [Filtering](https://docs.scipy.org/doc/scipy/reference/signal.html#filtering)
### [LFilter](https://docs.scipy.org/doc/scipy/reference/generated/scipy.signal.lfilter.html)
`scipy.signal.lfilter(b, a, x, ...)`

Filter data along one-dimension with an IIR or FIR filter.
* b
	* The numerator coefficient vector in a 1-D sequence
* a
	* The denominator coefficient vector in a 1-D sequence. If `a[0]` is not 1, then both _a_ and _b_ are normalized by `a[0]`.
* x
	* An N-dimensional input array.

> [!NOTE]+
> The function [`sosfilt`](https://docs.scipy.org/doc/scipy/reference/generated/scipy.signal.sosfilt.html#scipy.signal.sosfilt "scipy.signal.sosfilt") (and filter design using `output='sos'`) should be preferred over [`lfilter`](https://docs.scipy.org/doc/scipy/reference/generated/scipy.signal.lfilter.html#scipy.signal.lfilter "scipy.signal.lfilter") for most filtering tasks, as second-order sections have fewer numerical problems.

### [SOSFilt](https://docs.scipy.org/doc/scipy/reference/generated/scipy.signal.sosfilt.html#scipy.signal.sosfilt)
`scipy.signal.sosfilt(sos, x, ...)`

Filter data along one dimension using cascaded second-order sections.
Filter a data sequence, _x_, using a digital IIR filter defined by _sos_.
* sos
	* Array of second-order filter coefficients, must have shape `(n_sections, 6)`. Each row corresponds to a second-order section, with the first three columns providing the numerator coefficients and the last three providing the denominator coefficients.
* x
	* An N-dimensional input array.

### [FiltFilt](https://docs.scipy.org/doc/scipy/reference/generated/scipy.signal.filtfilt.html)
`scipy.signal.filtfilt(b, a, x, ...)`

Apply a digital filter forward and backward to a signal.

This function applies a linear digital filter twice, once forward and once backwards. The combined filter has zero phase and a filter order twice that of the original
* b
	* The numerator coefficient vector of the filter.
* a
	* The denominator coefficient vector of the filter. If `a[0]` is not 1, then both _a_ and _b_ are normalized by `a[0]`.
* x
	* The array of data to be filtered.

> [!NOTE]+
> The function [`sosfiltfilt`](https://docs.scipy.org/doc/scipy/reference/generated/scipy.signal.sosfiltfilt.html#scipy.signal.sosfiltfilt "scipy.signal.sosfiltfilt") (and filter design using `output='sos'`) should be preferred over [`filtfilt`](https://docs.scipy.org/doc/scipy/reference/generated/scipy.signal.filtfilt.html#scipy.signal.filtfilt "scipy.signal.filtfilt") for most filtering tasks, as second-order sections have fewer numerical problems.
