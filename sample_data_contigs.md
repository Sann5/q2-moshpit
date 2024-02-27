# `SampleData[Contigs]`
Artifact containing contigs, organized as one FASTA file per sample. Contigs are contiguous sequences of DNA that are assembled from overlapping reads.

## Artifact Format
```python
class ContigSequencesDirFmt(model.DirectoryFormat):
    sequences = model.FileCollection(
        r'[^\.].+_contigs.(fasta|fa)$',
        format=DNAFASTAFormat
    )
```

## Expected Folder Structure
```bash
data
├── <sample_id>_contigs.fa
⋮
└── <sample_id>_contigs.fa
```

## Where to find `SampleData[Contigs]`
- See [Generate MAGs from Reads](./generate_mags_from_reads.md) tutorial for a full example. 
- Bellow we list actions in `q2-moshpit` and `q2-assembly` that have this semantic type as output, input or both. 

### As Input
- `qiime assambly index-contigs`
- `qiime assambly evaluate-contigs`
- `qiime moshpit eggnog-diamond-search` # TODO: Link to md page
- [`qiime moshpit bin-contigs-metabat`](./bin-contigs-metabat.md)
- `qiime moshpit classify-kraken2` # TODO: Link to md page

### As Output
- `qiime assambly assemble-megahit`
- `qiime assambly assemble-spades`

### As Input and Output
- `qiime assambly partition-contigs`
- `qiime assambly collate-contigs`