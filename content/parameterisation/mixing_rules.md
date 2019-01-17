## Mixing rules

Generally these off-the-shelf potentials only give the van der Waals potential for a self interaction.
This is the interaction of a particular atom with another atom of the same type, e.g. an argon-argon interaction.
Therefore, it is necessary to determine how the different atom types **interact with one another**.
This is achieved through the application of mixing rules, providing a way to calculate the interaction potentials of different atoms interacting with each other.

One of the most common types of mixing rules are the **Lorentz-Berthelot** rules [[1, 2](#references)].
These are as follows,

$$ \sigma_{ij} = \dfrac{\sigma_{ii} + \sigma_{jj}}{2} \;\;\;\text{and}\;\;\; \varepsilon_{ij} = \sqrt{\varepsilon_{ii}\varepsilon_{jj}}. $$

The values $\sigma$ and $\varepsilon$ are from a slightly different formulation of the Lennard-Jones interaction,

$$ E(r_{ii}) = 4\varepsilon\Bigg[\bigg(\dfrac{\sigma_{ii}}{r_{ii}}\bigg)^{12} - \bigg(\dfrac{\sigma_{ii}}{r_{ii}}\bigg)^{6}\Bigg]. $$

This is simply an alternative way of writing the Lennard-Jones potential, as discussed previously.

As with the determination of the potentials itself, the way in which these potentials can be mixed vary massively and there is no single rule for all systems.
To give a flavour of the variation possible, the Wikipedia entry on [combining rules](https://en.wikipedia.org/wiki/Combining_rules) provides an introduction into some of the rule sets commonly employed.

## References

1. Lorentz, H. A. *Ann. Phys.* 1881, **248** (1), 1227–136. [10.1002/andp.18812480110](https://doi.org/10.1002/andp.18812480110).
2. Berthelot, D. *Comptes. Rendus. Acad. Sci.* 1898, **126**, 1703–1855.