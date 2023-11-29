# A71EV2A_iteration1_cmpd_order
# Compound upload for A71EV2A
## Iteration 1

Compounds were selected using a development version of a new pipeline that uses the Fragment Network to find follow-up compounds, primarily fragment merges and linker-like compounds. The original version of the pipeline is described in https://pubs.acs.org/doi/10.1021/acs.jcim.3c00276.

### Method

The new pipeline enumerates compatible pairs of substructures from pairs of fragment hits. The pairs of substructures are predicted to make an interaction (using ProLIF) and don't share a volume overlap of >30% (i.e. <30% the volume of one substructure overlaps with the other), to ensure they are 'compatible' substructures for merging.

The Fragment Network version we are using contains >200M compounds. The network is queried to find 'perfect merges' (i.e. merges containing exact substructures from the parent fragments) and 'bioisosteric merges', which incorporate similar substructures or substructures with different attachment points (using a 2D pharmacophore fingerprint to calculate similarity).

The naming convention includes: 
- Fragment A (source of the first substructure in the merge)
- Fragment B (source of the second substructure in the merge)
- An identifier (merges for each round are numbered)
- Three characters indicating the type of merge

  - First character: P (perfect merge) or B (bioisosteric merge)
  - Second character: if bioisosteric, 1 to indicate 1 substructure replaced or 2 to indicate 2 substructures replaced
  - Third character: if 2 rounds of bioisosteric merging, S indicates a strict search (no change to the linker from the 1st round of bioisosteric merging) or L indicates a looser search (allow change to the linker from the 1st round of bioisosteric merging)
- Some compounds also have rgrp-X at the end of the name, indicating that the compound was subjected to R-group expansion

### Filtering

Conformations are generated using Fragmenstein plus a custom mapping if bioisosteric merges and minimized using PyRosetta.

SDF files uploaded to Fragalysis are in the data directory. First file uploaded (compound-set_FragNetv"_A71EV2A_Iteration1_WithRGroups.sdf) looks only for merges between x0310_0A and x0556_0A.
Second file uploaded (compound-set_FragNetv2_A71EV2A_Iteration1_pt2.sdf) looks for merges between x0416_0A with x0310_0A and x0451_0A.

The first file includes all querying pipelines (perfect merging, 2 rounds of bioisosteric merging and R-group expansion). Filtering looks for compounds that maintain interactions seen for the parent fragments,
those with a SuCOS score > 0.5, with a combined RMSD < 1.5A, that make at least 2 interactions and 1 non-hydrophobic interaction. Diversity filter is also applied using Butina clustering and a distance threshold of 0.3 (calculated using Tanimoto with Morgan fp of 1,024 bits and radius 2).

The second file only includes perfect merging and 1 round of bioisosteric merging pipelines (the number of compounds found was much greater for these pairs). In addition to the above filtering, I also removed compounds with a mean overlap with the fragments of 55% and removed some manually with vary strainted conformations (my current filters aren't removing these and need to develop a good way to do this in an automated manner).
Thresholds were adjusted by eye to result in reasonable number (<300) of reasonable looking compounds and can be modified/finalised as the tool is developed.
