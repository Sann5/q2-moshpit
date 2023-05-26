# Usage tutorial
This is an introductory tutorial to shotgun metagenome analysis using the QIIME 2 MOSHPIT pipeline. 
This tutorial is still **work in progress** and will be updated regularly. It does not cover all aspects of the pipeline, 
but should provide a good starting point for new users. We will not be using any real data in this tutorial, but instead 
will simulate a small dataset based on a set of genomes of known origin.

## Prerequisites
You will need the conda environment created according to the instructions available under the [Installation](../Home#installation) section.
You should also download the genomes base on which we will simulate reads from [[here](https://github.com/bokulich-lab/q2-moshpit/wiki/genomes.qza)](https://github.com/bokulich-lab/q2-moshpit/wiki/genomes.qza):

```
curl -L -o genomes.qza https://github.com/bokulich-lab/q2-moshpit/wiki/genomes.qza
```