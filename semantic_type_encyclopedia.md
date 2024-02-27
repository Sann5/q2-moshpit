# Semantic Type Encyclopedia
"QIIME 2 uses a combination of types and formats to represent: what data means and how to use it." See [Qiime2 Documentation on Semantic Types](https://dev.qiime2.org/latest/storing-data/types/) for more information.

Bellow we list some semantic types that are specially relevant to `q2-moshpit`.

## `SampleData`
Genomic data organized by sample.

- [`SampleData[MAGs]`](./sample_data_mags.md)
- [`SampleData[Contigs]`](./sample_data_contigs.md)

## SampleData
### BLAST6
TODO: Description

#### Semantic Type
```
SampleData[BLAST6]
```

#### Artifact Format
```python
class SeedOrthologDirFmt(model.DirectoryFormat):
    seed_orthologs = model.FileCollection(
        r'.*\..*\.seed_orthologs',
        format=OrthologFileFmt,
        optional=False
    )
```

#### Expected Folder Structure
```
TODO
```

#### How to get `SampleData[BLAST6]`
See (link to `SampleData[Contigs]` and `FeatureData[MAG]`) to see how to get inputs.

1. Download or build Diamond DB
    - commands:
        - `build-custom-diamond_db`
        - `build_eggnog_diamond_db`
        - `fetch-diamond-db`
    - inputs:
        - see individual actions for inputs
    - outputs:
        - `ReferenceDB[Diamond]`
    
2. Find target sequences
    - command: `eggnog-diamond-search`
        - inputs: `SampleData[Contigs] | FeatureData[MAG]`, `ReferenceDB[Diamond]`
        - output: `SampleData[BLAST6]`, `FeatureTable[Frequency]`

## FeatureData
TODO: Description

### MAG
TODO: Description

#### Semantic Type
```
FeatureData[MAG]
```

#### Artifact Format
```python
class MAGSequencesDirFmt(model.DirectoryFormat):
    pathspec = (
        r"^[0-9a-fA-F]{8}-[0-9a-fA-F]{4}-4[0-9a-fA-F]{3}-"
        r"[89abAB][0-9a-fA-F]{3}-[0-9a-fA-F]{12}\.(fa|fasta)$"
    )

    sequences = model.FileCollection(pathspec, format=DNAFASTAFormat)
```

#### Expected Folder Structure
```
TODO
```

#### How to get `FeatureData[MAG]`
1. Decrepitate MAGs
    - command: `dereplicate-mags`
        - input: `SampleData[MAGs]`, `DistanceMatrix`
        - output: `FeatureData[MAG]`, `FeatureTable[PresenceAbsence]`


### NOG
TODO: Description

#### Semantic Type
```
FeatureData[NOG]
```

#### Artifact Format
```python
class OrthologAnnotationDirFmt(model.DirectoryFormat):
    annotations = model.FileCollection(
        r'.+\.annotations',
        format=OrthologFileFmt
    )
```

#### Expected Folder Structure
```
TODO
```

#### How to get `FeatureData[NOG]`
See (link to `SampleData[BLAST6]`) to see how to get the `SampleData[BLAST6]` input. 

1. Download eggNOG database
    - command: `eggnog-fetch-db`
        - output: `ReferenceDB[Eggnog]`
1. Annotate orthologs
    - command: `eggnog_annotate`
        - input: `SampleData[BLAST6]`, `ReferenceDB[Eggnog]`
        - output: `FeatureData[NOG]`

## FeatureTable
TODO: Description

### Frequency
A feature table (e.g., samples by ASVs) where each value in the matrix is a whole number greater than or equal to 0 representing the frequency or count of a feature in the corresponding sample. These data should be raw (not normalized) counts.

#### Semantic Type
```
FeatureTable[Frequency]
```

#### Artifact Format
```python
BIOMV210DirFmt = model.SingleFileDirectoryFormat(
    'BIOMV210DirFmt',
    'feature-table.biom',
    BIOMV210Format
)
```

#### Expected Folder Structure
```bash
data
└── feature-table.biom
```

#### How to get `FeatureTable[Frequency]`
- commands: 
    - `eggnog-diamond-search`
    - `classify-kaiju`
    - `estimate-bracken`
- input: se individual actions for information on the inputs
- output: `FeatureTable[Frequency]`