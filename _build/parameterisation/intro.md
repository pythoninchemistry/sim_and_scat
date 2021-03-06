---
interact_link: content/parameterisation/intro.ipynb
title: 'Parameterisation'
prev_page:
  url: /classical_methods/potential_models
  title: 'Potential models'
next_page:
  url: /parameterisation/off_the_shelf
  title: 'Off-the-shelf potentials'
comment: "***PROGRAMMATICALLY GENERATED, DO NOT EDIT. SEE ORIGINAL FILES IN /content***"
---

# Parameterisation

Once the potential energy function to be used for a particular interaction has been determined, it is then necessary to **parameterise** the function. 

If we consider the parameterisation Lennard-Jones potential model.
In this model it is necessary to determine two parameters, $\sigma$ and $\varepsilon$. 
$\sigma$ is the distance at which the potential energy between the two particles is zero, $-\varepsilon$ is the potential energy at the equilbrium separation. 
Values for each of these must be determined for each pair of atoms in our system.

## How to parameterise a potential model?

The purpose of parameterisation is to develop a potential energy model that is able to **accurately reproduce** the relative energy of a given interaction. 
This may also be thought of as the model that reproduces the structure accurately. 
Parameters should really be obtained by optimising them with respect to a **more accurate** technique than classical simulation. 
Commonly, this involves either experimental measurements, e.g. X-ray crystallography, or quantum mechanical calculations; we will focus on the latter. 

More can be found out about quantum mechanical calculations in the textbooks mentioned in the introduction (in particular Jeremy Harvey's Computational Chemistry Primer [[1](#references)]).
However, for our current purposes we only need to remember that quantum calculations are more accurate than classical simulations.

### Parameterising a Lennard-Jones interaction

We will stick with the example of a Lennard-Jones interaction, however the arguments and methods discussed are **extensible to all different interaction types**. 
To generate the potential energy model between two particles of argon, we could conduct quantum mechanical calculations at a range of inter-atom separations, from 2 to 5 Å, finding the energy between the two particles at each separation.

The Python code below plots the energy against distance that has been obtained from a quantum mechanical calculation. 



{:.input_area}
```python
import matplotlib.pyplot as plt
import numpy as np

r = np.arange(3.5, 7., 0.5)
energy = np.array([0.1374, -0.0195, -0.0218, 
                   -0.0133, -0.0076, -0.0043, 
                   -0.0025])
energy_err = energy * 0.1

plt.errorbar(r, energy, yerr=energy_err, 
             marker='o', ls='')
plt.xlabel(r'$r$/Å')
plt.ylabel(r'$E$/eV')
plt.show()
```



{:.output .output_png}
![png](../images/parameterisation/intro_2_0.png)



We can already see that the general shape of the curve is similar to a Lennard-Jones (or Buckingham) interaction.
There is a well near the **equilibrium bond distance** and a steep incline as the particles come close together. 
It is possible to then fit a Lennard-Jones function to this data, the Python code below so using a simple least-squares fit.



{:.input_area}
```python
from scipy.optimize import curve_fit

def lj_energy(r, epsilon, sigma):
    """
    Implementation of the Lennard-Jones potential 
    to calculate the energy of the interaction.
    
    Parameters
    ----------
    r: float
        Distance between two particles (Å)
    epsilon: float 
        Potential energy at the equilibrium bond 
        length (eV)
    sigma: float 
        Distance at which the potential energy is 
        zero (Å)
    
    Returns
    -------
    float
        Energy of the van der Waals interaction (eV)
    """
    return 4 * epsilon * np.power(
        sigma / r, 12) - 4 * epsilon * np.power(
        sigma / r, 6)

popt, pcov = curve_fit(lj_energy, r, energy, 
                       sigma=energy_err)
print('Best value for ε = {:.2e} eV'.format(
    popt[0]))
print('Best value for σ = {:.2f} Å'.format(
    popt[1]))
```


{:.output .output_stream}
```
Best value for ε = 2.02e-02 eV
Best value for σ = 3.81 Å

```

These values are similar to those from Rahman [[2](#References)].
However, the agreement can be more easily assessed with by plotting the Lennard-Jones function with the values fitted and the quantum mechnical data together.
These values agree with many datapoints, although it is clear that at short distances it would be necessary to perform further quantum mechanical calculations. 



{:.input_area}
```python
plt.errorbar(r, energy, yerr=energy_err, marker='o', ls='')
x = np.linspace(3.5, 7, 1000)
plt.plot(x, lj_energy(x, popt[0], popt[1]))
plt.xlabel(r'$r$/Å')
plt.ylabel(r'$E$/eV')
plt.show()
```



{:.output .output_png}
![png](../images/parameterisation/intro_6_0.png)



Note that it would be necessary to carry out this process for **every** interaction in your system, e.g. bond lengths, bond angles, dihedral angles, van der Waals and Coulombic interactions forces, etc. 
Furthermore, it is important to remember the **different chemistry** that is present for each atom. 
For example, a carbon atom in a carbonyl group will not display the same behaviour as the carbon atom in a methane molecule. 
To carry out these calculations for *every* molecular dynamics simulation that you wish to perform very quickly becomes highly unfeasible if we want to apply classical simulation regularly.

## References

1. Harvey, J. *Computational Chemistry*; Oxford University Press: Oxford, 2018.
2. Rahman, A. *Phys. Rev.* 1964, **136** (2A), A405–A411. [10.1103/PhysRev.136.A405](https://doi.org/10.1103/PhysRev.136.A405).
