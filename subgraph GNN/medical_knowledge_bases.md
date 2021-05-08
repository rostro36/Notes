# Medical knowledge bases

Knowledge bases often are noisy, especially if they are mined e.g. from literature:
## DDI
Used in [sumGNN](https://arxiv.org/abs/2010.01450):
[Hetionet](https://het.io/)
[DRKG](https://github.com/gnn4dr/DRKG)
[Google Health knowledge](https://www.nature.com/articles/s41598-017-05778-z)

Curated, smaller, but more consistent:
## DDI
Used in [sumGNN](https://arxiv.org/abs/2010.01450):
- [TWOSIDES](http://tatonettilab.org/offsides/) &rightarrow; 200 relation labels
- [MINER](https://github.com/snap-stanford/miner-data)
- [DrugBank](https://go.drugbank.com/) &rightarrow; one edge type

## Combined
[SPOKE](https://spoke.ucsf.edu/) used [here](https://www.nature.com/articles/s41467-019-11069-0)
[Pubmed Knowledge Graph](https://www.nature.com/articles/s41597-020-0543-2)
## SNAP
[SNAP \(Stanford\)](http://snap.stanford.edu/biodata/)
- **Cell** &rightarrow; basic structural, biological, and functional unit of all organisms measured by single-cell technologies
- **Disease** &rightarrow; medical condition that is associated with specific symptoms and signs
- **Drug/Chemical** &rightarrow; chemical substance of known structure that produces a biological effect
- **Function** &rightarrow; gene role classified into molecular functions, cellular components, and biological processes
- **Gene** &rightarrow; sequence of DNA or RNA that codes for a molecule that has a function
- **Genomic region** &rightarrow; segment of a nucleic acid molecule, e.g., a regulatory sequence
- **Protein** &rightarrow; molecule that performs a vast array of functions within organisms
- **Side-effect** &rightarrow; secondary, typically undesirable effect of a drug or medical treatment
- **Species/Organism** &rightarrow; basic unit of classification and a taxonomic rank, as well as a unit of biodiversity
- **Tissue** &rightarrow; cellular organizational level between cells and a complete organ

## EHR
[MIMIC-III](https://physionet.org/content/mimiciii/1.4/)
## Labels
[SNOMED](https://www.snomed.org/)
- organisms, procedures, substances, disorders, finding, observable entities, situation, drugs
[CPT](https://www.aapc.com/codes/cpt-codes-range/)
- Current Procedural Terminology, terminology for cures
[LOINC](https://loinc.org/)
- health measurements, observations, documents
[UMLS](https://www.nlm.nih.gov/research/umls/index.html)
- also good conversion
[MeSH](https://www.nlm.nih.gov/mesh/meshhome.html)
- **C** &rightarrow; Diseases
- **D** &rightarrow; Chemicals and Drugs
- **E** &rightarrow; Analytical, Diagnostic and Therapeutic Techniques, and Equipment
