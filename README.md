# SampleEntropy

fMRI displays complex BOLD signal

We have looked at this using the Hurst exponent, but Entropy may be another method: [Example](https://www.frontiersin.org/articles/10.3389/fnins.2020.00700/full)

[Wikipedia Article](https://en.wikipedia.org/wiki/Sample_entropy)

## Example code
```python
import numpy as np

def sampen(L, m, r):
    N = len(L)
    B = 0.0
    A = 0.0
    
    
    # Split time series and save all templates of length m
    xmi = np.array([L[i : i + m] for i in range(N - m)])
    xmj = np.array([L[i : i + m] for i in range(N - m + 1)])

    # Save all matches minus the self-match, compute B
    B = np.sum([np.sum(np.abs(xmii - xmj).max(axis=1) <= r) - 1 for xmii in xmi])

    # Similar for computing A
    m += 1
    xm = np.array([L[i : i + m] for i in range(N - m + 1)])

    A = np.sum([np.sum(np.abs(xmi - xm).max(axis=1) <= r) - 1 for xmi in xm])

    # Return SampEn
    return -np.log(A / B)
```

I believe [Nezafati](https://www.frontiersin.org/articles/10.3389/fnins.2020.00700/full) used m = 3, and r = 0.6*STD
