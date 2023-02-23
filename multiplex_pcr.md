# Clark / Campino Group SOP 
## Multiplex PCR design, optimization and implementation
### Document version: 1.0
##### Description: A protocol for the implementation of a single-tube multiplex PCR reaction for amplicon sequencing of pathogen DR loci.

This protocol covers the PCR reaction of the Clark/Campino amplicon sequencing workflow

Below is a guide to:
1) Optimise primer compatibility using a checkerboard PCR analysis.
2) Mix primers, maintaining the correct molarity of each.
3) Make a PCR mastermix for single tube amplification.

### 1) Checkerboard PCR
Firstly, identify 'problem primers' by mixing them all in an equimolar reaction, as shown in __step 3__. Amplicons with reduced or no coverage and either be re designed, or optimised using the below protocol. In this example, we screen 4 primer sets (on the y-axis of the matrix) against the entire panel of 8 primers.

To optimise 8 primer (Fwd+Rev) sets, set up stock mixtures of double the required working concentration in first column and first row of a 96-well plate. Build a matrix of mixtures on the plate as shown below, and then transfer the duplex primer mixtures to a mirror setup, instead with a PCR mastermix.

| Primer stocks | 1  | 2   | 3   | 4   | 5   | 6   | 7   | 8   |
| ------------- | -- | --- | --- | --- | --- | --- | --- | --- |
| 1             | NA | 1+2 | 1+3 | 1+4 | 1+5 | 1+6 | 1+7 | 1+8 |
| 2             |    | NA  | 2+3 | 2+4 | 2+5 | 2+6 | 2+7 | 2+8 |
| 3             |    |     | NA  | 3+4 | 3+5 | 3+6 | 3+7 | 3+8 |
| 4             |    |     |     | NA  | 4+5 | 4+6 | 4+7 | 4+8 |

Here, we run 32 PCR reactions to ascertain primers which form homo/hetro dimers, reducing the efficiency fot he reaction. The reactions can be visualized on a gel or sequenced, indicating exactly which combinations are affected and therefor need to be re designed.

### 2) Primer mixture
Mix the primers so that each Fwd and Rev is maintained at the correct working concentration. In the case of NEB Q5 Polymerase, this is 0.5 µM each. If starting from the 100 µM stock, 32 primers diluted in oneanother in an equimolar fashion would require 3.2 µL. This is a significantly large quantity of primer, and should be optimised for the assay.


### 3) Multiplex PCR Reaction

Set up a standard PCR reaction as per manufacturer's instruction.

| COMPONENT                       | 25 µl REACTION | FINAL CONCENTRATION |
|---------------------------------|----------------|---------------------|
| 5X Q5 Reaction Buffer           | 5 µl           | 1X                  |
| 10 mM dNTPs                     | 0.5 µl         | 200 µM              |
| Primer multiplex mixture        | variable µl    | 0.5 µM each         |
| Template DNA                    | variable       | < 1,000 ng          |
| Q5 High-Fidelity DNA Polymerase | 0.25 µl        | 0.02 U/µl           |
| Nuclease-Free Water             | to 25 µl       |                     |

| STEP                 | TEMP        | TIME       |
| -------------------- | ----------- | ---------- |
| Initial Denaturation | 98°C        | 30 seconds |
| 35 cycles            | 98°C        | 5 seconds  |
| 65                   | 15 seconds  |
| 72°C                 | 120 seconds |
| Final Extension      | 72°C        | 2 minutes  |
| Hold                 | 4–10°C      |            |

md>
