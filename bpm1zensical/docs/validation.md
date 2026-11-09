---
icon: lucide/check
---
# Validation

Before using the simulations, we checked that the code gives correct results. For each test we
compare the BPM with a result that does not come from the BPM: an analytical formula or an
independent mode solver. Each test checks a different part of the code.

| Test | What it checks | Difference with theory |
|---|---|---|
| 1. Free space | the diffraction step (FFT) | $< 10^{-5}$ |
| 2. Coupling length | the index step and the coupling | $\leq 1\,\%$ |
| 3. Mach-Zehnder | a profile that changes with $z$ | $\leq 2\,\%$ |

The tests 1 and 2 are in `validation.py`:

```bash
python validation.py
```

## 1. Free space

**What we check.** Without guide ($\Delta n = 0$), only the diffraction step of the split-step
method is used. A Gaussian beam stays Gaussian and its width must follow

$$
w(z) = w_0\sqrt{1 + \left(\frac{z}{z_R}\right)^2}, \qquad z_R = \frac{\pi n_0 w_0^2}{\lambda_0}
$$

We measure the width of the simulated beam from its intensity profile and compare it with
this formula.

![Validation free space](images/validation_free_space.png)

| $w_0$ | $z_R$ | maximum error |
|---|---|---|
| 3 µm | 18.2 µm | $2 \times 10^{-6}$ |
| 10 µm | 202.7 µm | $< 10^{-14}$ |

**Result.** The simulation and the theory are the same: in free space the method is exact.

## 2. Coupling length

**What we check.** The index step and the coupling between two guides
($W = 4$ µm, $\Delta n_{max} = 0.01$).

The theory of coupled guides gives

$$
L_c = \frac{\pi}{\beta_{even} - \beta_{odd}}
$$

where $\beta_{even}$ and $\beta_{odd}$ are the propagation constants of the two modes of the
coupler. We compute them with a **mode solver** (finite differences, `scipy.sparse.linalg.eigsh`),
which is independent of the BPM. In the BPM, $L_c$ is the position of the first maximum of power
in guide 2.

![Validation coupling length](images/validation_coupling.png)

| Gap | $L_c$ theory | $L_c$ BPM | Error |
|---|---|---|---|
| 3 µm | 816 µm | 813 µm | 0.4 % |
| 4 µm | 1374 µm | 1373 µm | 0.04 % |
| 5 µm | 2308 µm | 2331 µm | 1.0 % |
| 6 µm | 3875 µm | 3896 µm | 0.5 % |

**Result.** The error is 1 % or less, while $L_c$ changes by a factor of 5. The small difference
comes from the input Gaussian beam, which is not exactly the mode of the guide.

## 3. Mach-Zehnder

**What we check.** A structure where the profile $\Delta n(x, z)$ changes along $z$
(S-bends and phase shifter).

With 50/50 couplers, the power in the bottom output is

$$
P_{bottom} = \sin^2\left(\frac{\Gamma\, k_0\, \delta n\, L_{arm}}{2}\right)
$$

where $\Gamma = 0.769$ is the confinement factor: the part of the mode that is inside the guide
and sees the phase shifter $\delta n$.

![Validation Mach-Zehnder](images/validation_mzi.png)

| $\delta n$ | $\Delta\varphi$ | theory | BPM |
|---|---|---|---|
| 0 | 0 | 0 % | 0.4 % |
| $2.52 \times 10^{-4}$ | $\pi/2$ | 50 % | 50.1 % |
| $5.04 \times 10^{-4}$ | $\pi$ | 100 % | 99.4 % |
| $7.56 \times 10^{-4}$ | $3\pi/2$ | 50 % | 48.1 % |
| $1.01 \times 10^{-3}$ | $2\pi$ | 0 % | 0.6 % |

**Result.** The BPM follows the theory with $\Gamma$ (difference at most 2 %). Without $\Gamma$
(gray dashed line) the theory does not work.

## Conclusion

In all the tests the difference with the theory is about 1 % (at most 2 %). The code is correct,
so we can trust the simulations of the [Results](results.md) page.
