# `bin-contigs-metabat`
This method uses MetaBAT 2 to bin provided contigs into MAGs. 

> For the full description of the action inputs, outputs and parameters run `qiime moshpit bin-contigs-metabat --help` in the terminal.

## Inputs
- `contigs`: Contigs to be binned. (format: `SampleData[Contigs]`)
- `alignment-maps` Reads-to-contig alignment maps. (format: `SampleData[AlignmentMap]`)

## Outputs
- `mags`: The resulting MAGs. (format: `SampleData[MAGs]`)
- `contig-map`: Mapping of MAG identifiers to the contig identifiers contained in each MAG. (format: `FeatureMap[MAGtoContigs]`)
- `unbinned-contigs`: Contigs that were not binned into any MAG. (format: `SampleData[Contigs % Properties('unbinned')]`)


## Citation
```
@article{key0,
 author = {Kang, Dongwan D. and Li, Feng and Kirton, Edward and Thomas, Ashleigh and Egan, Rob and An, Hong and Wang, Zhong},
 doi = {10.7717/peerj.7359},
 issn = {21678359},
 journal = {PeerJ},
 keywords = {Clustering,Metagenome binning,Metagenomics},
 number = {7},
 publisher = {PeerJ Inc.},
 title = {MetaBAT 2: An Adaptive Binning Algorithm for Robust and Efficient Genome Reconstruction from Metagenome Assemblies},
 volume = {2019},
 year = {2019}
}
```