# Usage tutorial
This is an introductory tutorial to shotgun metagenome analysis using the QIIME 2 MOSHPIT pipeline. 
This tutorial is still **work in progress** and will be updated regularly. It does not cover all aspects of the pipeline, 
but should provide a good starting point for new users. We will not be using any real data in this tutorial, but instead 
will simulate a small dataset based on a set of genomes of known origin.

## Prerequisites
You will need the conda environment created according to the instructions available under the [Installation](../Home#installation) section.
You should also download the genomes based on which we will simulate reads from [here](https://github.com/bokulich-lab/q2-moshpit/wiki/genomes.qza):

```shell
curl -L -o genomes.qza https://github.com/bokulich-lab/q2-moshpit/wiki/genomes.qza
```

Note on the runtime estimates: these are based on a 10-core machine with 96GB of RAM.

## Simulating reads
To simulate reads, we can use the `generate-reads` command from the [q2-assembly](https://github.com/bokulich-lab/q2-assembly) plugin. 
We can specify how many samples should be generated with home many reads, and which abundance distributions. 
For now, let's simulate 4 samples with 1000000 reads (uniform abundance distribution for 10 random genomes):
```shell
qiime assembly generate-reads \
  --i-genomes genomes.qza \
  --p-sample-names sample1 sample2 sample3 sample4 \
  --p-n-reads 1000000 \
  --p-abundance uniform \
  --p-n-genomes 10 \
  --p-cpus 10 \
  --output-dir reads \
  --verbose
```
Estimated runtime: 15-20 minutes

## Metagenome assembly & QC
### Assembly
We can use the simulated reads to perform metagenome assembly. There are two assemblers available in the [q2-assembly](https://github.com/bokulich-lab/q2-assembly)
plugin: we will use the `MEGAHIT` assembler in this tutorial - feel free to try out the `MetaSPAdes` assembler though.
```shell
qiime assembly assemble-megahit \
  --i-seqs reads/reads.qza \
  --p-presets meta-sensitive \
  --p-num-cpu-threads 10 \
  --o-contigs contigs.qza \
  --verbose
```
Estimated runtime: x minutes

### Contig QC


## Genome binning
### Contig indexing
### Mapping reads to contigs
### MAG generation
