# `SampleData[MAGs]`
`SampleData[MAGs]` represents artifacts containing MAGs, organized by sample in folders. Individual MAGs are `DNAFASTAFormat` files. The MANIFEST is a table specifying the relationship MAGs and samples.

## Artifact Format
```python
class MultiMAGSequencesDirFmt(MultiFASTADirectoryFormat):
    sequences = model.FileCollection(r'.+\.(fa|fasta)$', format=DNAFASTAFormat)
    manifest = model.File('MANIFEST', format=MultiMAGManifestFormat)
```

## Expected Folder Structure
```bash
data
├── MANIFEST
├── <sample_id>
│   ├── <mag_id>.fasta
│   ⋮
│   └── <mag_id>.fasta
⋮
└── <sample_id>
    ├── <mag_id>.fasta
    ⋮
    └── <mag_id>.fasta
```

## Where to find `SampleData[MAGs]`
- See [Generate MAGs from Reads](./generate_mags_from_reads.md) tutorial for a full example. 
- Bellow we list actions in `q2-moshpit` that have this semantic type as output or input. 

### As Input
- `qiime moshpit dereplicate-mags` # TODO: Link to md page
- `qiime moshpit evaluate-busco` # TODO: Link to md page

### As Output
- [`qiime moshpit bin-contigs-metabat`](./bin-contigs-metabat.md)