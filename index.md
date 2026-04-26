---
layout: default
title: Home
---

# Welcome to my Technical Page

### Engineering Formula Example
The characteristic impedance of a transmission line can be expressed as:

$$Z_0 = \sqrt{\frac{R + j\omega L}{G + j\omega C}}$$

### Code Example
```python
def vswr_calc(forward, reflected):
    rho = (reflected / forward)**0.5
    return (1 + rho) / (1 - rho)
