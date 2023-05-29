# MOSHPIT
**MO**dular **SH**otgun metagenome **P**ipelines with **I**ntegrated provenance **T**racking

This page contains installation instructions for the MOSHPIT project - this is still **work in progress**!

## Components 
The figure below illustrates actions available (green) and being under development (grey).
![MOSHPIT components](img/pipeline_overview.png | width=300)

You can find more information about the individual components in the respective repositories:

|      Plugin |                                                                                     Source |
|:------------|:-------------------------------------------------------------------------------------------|
| q2-assembly | [https://github.com/bokulich-lab/q2-assembly](https://github.com/bokulich-lab/q2-assembly) |
| q2-moshpit  | [https://github.com/bokulich-lab/q2-moshpit](https://github.com/bokulich-lab/q2-moshpit)   |
| q2-checkm   | [https://github.com/bokulich-lab/q2-checkm](https://github.com/bokulich-lab/q2-checkm)     |


## Installation   
Before you proceed, make sure you have [mamba](https://mamba.readthedocs.io/en/latest/installation.html) available in your base environment. Then, to install all the required components, execute:   
```shell
mamba create -yn q2-shotgun \
    -c conda-forge -c bioconda -c https://packages.qiime2.org/qiime2/2023.5/tested -c defaults \
    q2cli q2-assembly q2-moshpit q2-checkm
```
There is an issue with QUAST which requires a (hopefully) temporary fix:
```shell
conda run -n q2-shotgun \
    pip install --no-deps --force-reinstall git+https://github.com/misialq/quast.git@issue-230
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
