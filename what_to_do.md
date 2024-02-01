# What have you worked on?
## BUSCO
- Fetch database and run BUSCO, which quantifies the quality of the input MAGs based on the presence of single copy orthologs.
- **Input:** `SampleData[MAGs]`
- **Output:** visualization summarizing the quality of the input MAGs
- Commands: 
    1. `fetch-busco-db` 
    2. `evaluate-busco`

> Questions:
> - What is `SampleData[MAGs]`?

## eggNOG
### Find target sequences to annotate
- Download data, build Diamond database and search for possible target sequences to annotate.
- **Input:** `SampleData[Contigs] | FeatureData[MAG]`
- **Output:** `SampleData[BLAST6] & FeatureTable[Frequency]`
- Commands: 
    - `fetch-diamond-db` -> `eggnog-diamond-search`
    - `fetch-taxonomy-db` -> `build-custom-diamond-db` -> `eggnog-diamond-search`
    - `fetch-eggnog-proteins` -> `build-eggnog-diamond-db` -> `eggnog-diamond-search`

> Questions:
>  - Is the description of `eggnog-diamond-search` correct?
> - Why would we want to find these genes?
> - What is `SampleData[Contigs]`?
> - What is `FeatureData[MAG]`?
> - What is `SampleData[BLAST6]`?
> - What is `FeatureTable[Frequency]`?
> - Is the output of this actions the input to `eggnog-annotate`

### Annotate
- Download database and annotate seed orthologs against eggNOG database.
- **Input:** `SampleData[BLAST6]`
- **Final output:** `FeatureData[NOG]` artifact containing the annotations
- Commands:
    1. `fetch-eggnog-db`
    2. `eggnog-annotate`

> Questions:
> - Why would we want the annotation?
> - What is `SampleData[BLAST6]`?
> - What is `FeatureData[MAG]`?
> - What is `SampleData[NOG]`?

## Prodigal
- Predict genes from MAGs with Prodigal (PROkaryotic DYnamic programming Gene-finding ALgorithm), a gene prediction algorithm.
- **Input:** `FeatureData[MAG]`
- **Output:** `GenomeData[Proteins]` & `GenomeData[Loci]` & `GenomeData[Genes]`
- Commands:
    - `predict_genes_prodigal`

> Questions:
> - how do you get a `FeatureData[MAG]`?
> - What is the difference between `FeatureData[MAG]` and `SampleData[MAG]`?