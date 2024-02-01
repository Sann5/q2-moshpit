# Semantic Type Encyclopedia
## SampleData
TODO: Description

### Contigs
Artifact containing contigs, organized as one FASTA file per sample. Contigs are contiguous sequences of DNA that are assembled from overlapping reads.

#### Semantic Type
```
SampleData[Contigs]
```

#### Artifact Format
```python
class ContigSequencesDirFmt(model.DirectoryFormat):
    sequences = model.FileCollection(
        r'[^\.].+_contigs.(fasta|fa)$',
        format=DNAFASTAFormat
    )
```

#### Expected Folder Structure
```bash
data
├── <sample_id>_contigs.fa
├── <sample_id>_contigs.fa
└── <sample_id>_contigs.fa
```

#### How to obtain a `SampleData[Contigs]`
See (`moshpit` Tutorial) from full example.
1. Use reads from experiment or simulate reads from genomes.
    - command: `qiime assembly generate-reads`
        - input: `genomes.qza`
        - output: `reads.qza`
2. Assemble contigs
    - command: `qiime assembly assemble-megahit`
        - input: `reads.qza`
        - output: `contigs.qza`

### MAGs
Artifact containing MAGs, organized by sample in folders. Individual MAGs are `DNAFASTAFormat` files. The MANIFEST is a table specifying the relationship MAGs and samples.

#### Semantic Type
```
SampleData[MAGs]
```

#### Artifact Format
```python
class MultiMAGSequencesDirFmt(MultiFASTADirectoryFormat):
    sequences = model.FileCollection(r'.+\.(fa|fasta)$', format=DNAFASTAFormat)
    manifest = model.File('MANIFEST', format=MultiMAGManifestFormat)
```

#### Expected Folder Structure
```bash
data
├── MANIFEST
├── <sample_id>
│   ├── <mag_id>.fasta
│   ├── <mag_id>.fasta
│   └── <mag_id>.fasta
.

└── <sample_id>
    ├── <mag_id>.fasta
    ├── <mag_id>.fasta
    └── <mag_id>.fasta
```

#### How to obtain a `SampleData[MAGs]`
See `moshpit` Tutorial (TODO: link to tutorial) from full example. Alternatively, see (TODO: link to `SampleData[Contigs]`) to see how to obtain `contigs.qza` which is a `SampleData[Contigs]` artifact.

1. Index contigs
    - command: `qiime assembly index-contigs`
        - input: `contigs.qza` (`SampleData[Contigs]`)
        - output: `contigs-index.qza`
2. Mapping reads to contigs
    - command: `qiime assembly map-reads-to-contigs`
        - input: `contigs.qza`, `contigs-index.qza`
        - output: `mags.qza` (`SampleData[MAGs]`)

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