# Generate MAGs from Reads
In this workflow tutorial we will be transforming raw sequencing reads into Metagenome-Assembled Genomes (MAGs) using Qiime. Before you start make sure you have a working virtual environment (instructions available [here](../Home#installation)).

> Approximate runtime:  ??min  
(Estimate based on runtime of MacBook Pro with 8 intel cores and 16 RAM)

## Simulating Reads
We will not be using any real data in this tutorial, but instead will simulate a small dataset based on a set of genomes of known origin. However feel free to skip this step if you have some read data you would like to apply this workflow to. Check out [Qiime2 documentation](https://docs.qiime2.org/2024.2/tutorials/importing/#:~:text=files%20is%2033.-,PairedEndFastqManifestPhred64V2,-%C2%B6) to find out how to import your reads into Qiime.

To simulate reads, we can use the `generate-reads` command from the [q2-assembly](https://github.com/bokulich-lab/q2-assembly) plugin. 
We can specify how many samples should be generated with home many reads, and which abundance distributions. 
For now, let's simulate 3 samples with 20000 reads (uniform abundance distribution for 3 random genomes):

> Estimated runtime: ?? minutes

```shell
# Download genomes
cd <download_here>
curl -L -o genomes.qza https://github.com/bokulich-lab/q2-moshpit/wiki/genomes.qza

# Simulate reads
qiime assembly generate-reads \
  --i-genomes genomes.qza \
  --p-sample-names sample1 sample2 sample3 \
  --p-n-reads 2000000 \
  --p-abundance uniform \
  --p-n-genomes 3 \
  --p-cpus 10 \
  --output-dir reads \
  --verbose
```

## Metagenome Assembly & QC
### Assembly
We can use the simulated reads to perform metagenome assembly. There are two assemblers available in the [q2-assembly](https://github.com/bokulich-lab/q2-assembly)
plugin: we will use the `MEGAHIT` assembler in this tutorial - feel free to try out the `MetaSPAdes` assembler though.

> Estimated runtime: ?? minutes

```shell
qiime assembly assemble-megahit \
  --i-seqs reads/reads.qza \
  --p-presets meta-sensitive \
  --p-num-cpu-threads 10 \
  --o-contigs contigs.qza \
  --verbose
```

### Contig QC
> Estimated runtime: ?? minutes

```shell
qiime assembly evaluate-contigs \
  --i-contigs contigs.qza \
  --p-min-contig 1000 \
  --p-threads 10 \
  --o-visualization contigs.qzv \
  --verbose
```

## MAG Generation
Before we perform the actual binning (MAG generation), we will need to map the reads to the 
assembled contigs. The resulting alignment map can then be used directly in 
the binning action.

### Contig Indexing
We begin by generating a Bowtie2 index of the assembled contigs. This can be 
achieved by using the `index-contigs` action from the [q2-assembly](https://github.com/bokulich-lab/q2-assembly) 
plugin:

> Estimated runtime: 5 minutes

```shell
qiime assembly index-contigs \
  --i-contigs contigs.qza \
  --p-threads 10 \
  --p-seed 100 \
  --o-index contigs-index.qza \
  --verbose
```

### Mapping Reads to Contigs
Next, we will generate a reads-to-contigs alignment map using the `map-reads-to-contigs` action from 
[q2-assembly](https://github.com/bokulich-lab/q2-assembly):

> Estimated runtime: ? minutes

```shell
qiime assembly map-reads-to-contigs \
  --i-indexed-contigs contigs-index.qza \
  --i-reads reads/reads.qza \
  --p-threads 10 \
  --p-seed 100 \
  --o-alignment-map reads-to-contigs-aln.qza \
  --verbose
```

### MAG Generation
Finally, we are ready to perform contig binning using [MetaBAT2](https://peerj.com/articles/7359/) through the [`bin-contigs-metabat`](./bin-contigs-metabat.md) action from 
[q2-moshpit](https://github.com/bokulich-lab/q2-moshpit):

> Estimated runtime: ? minutes

```shell
qiime moshpit bin-contigs-metabat \
  --i-contigs contigs.qza \
  --i-alignment-maps reads-to-contigs-aln.qza \
  --p-num-threads 10 \
  --p-seed 100 \
  --o-mags mags.qza \
  --verbose
```

## Related tutorials
Once you have obtained your MAGs you can use `q2-moshpit` to do quality control, gene prediction or functional annotation.

- [MAG Quality Control](./mag_quality_control.md)
- [Annotate MAGs contigs](./annotate_mags_or_contigs.md)
- [Predict Genes from MAGs](./predict_genes_from_mags.md)


