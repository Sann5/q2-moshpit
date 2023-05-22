# MOSHPIT
**MO**dular **SH**otgun metagenome **P**ipelines with **I**ntegrated provenance **T**racking

This page contains installation instructions for the MOSHPIT project - this is still **work in progress**!

## Components   
|      Plugin |                                                                                     Source |
|:------------|:-------------------------------------------------------------------------------------------|
| q2-assembly | [https://github.com/bokulich-lab/q2-assembly](https://github.com/bokulich-lab/q2-assembly) |
| q2-moshpit  | [https://github.com/bokulich-lab/q2-moshpit](https://github.com/bokulich-lab/q2-moshpit)   |
| q2-checkm   | [https://github.com/bokulich-lab/q2-checkm](https://github.com/bokulich-lab/q2-checkm)     |

## Installation   
Before you proceed, make sure you have [mamba](https://mamba.readthedocs.io/en/latest/installation.html) available in your base environment. Then, to install all the required components, execute:   
```
mamba create -yn q2-shotgun \
    -c conda-forge -c bioconda -c https://packages.qiime2.org/qiime2/2023.5/tested -c defaults \
    q2cli q2-assembly q2-moshpit q2-checkm
```
This will create the _q2-shotgun_ QIIME 2 environment, which you can then activate by:   
```
conda activate q2-shotgun
```
Once active, refresh cache (once) and see which plugins are available:   
```
qiime dev refresh-cache
qiime info
```

## Issues
If you encounter any issues or if there is any feedback you would like to provide, please use the issue tracker in the repository of the respective plugin (see table above). Thanks!
