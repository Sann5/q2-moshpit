### MAG quality control
We can now use the `evaluate-bins` action from [q2-moshpit](https://github.com/bokulich-lab/q2-moshpit) to evaluate 
the quality of the generated MAGs. This action uses CheckM to estimate the completeness and contamination of the 
generated MAGs. Before we can run the evaluation itself, we need to download the CheckM database first:
```shell
curl -L -o checkm-db.tar.gz https://data.ace.uq.edu.au/public/CheckM_databases/checkm_data_2015_01_16.tar.gz
mkdir checkm_db
tar -xzf checkm-db.tar.gz -C checkm_db
rm checkm-db.tar.gz
```
Now, run the binning evaluation:
```shell
qiime checkm evaluate-bins \
  --i-bins mags.qza \
  --p-db-path ./checkm_db \
  --p-reduced-tree \
  --p-threads 10 \
  --p-pplacer-threads 10 \
  --o-visualization mags.qzv \
  --verbose
```
Estimated runtime: 30 minutes

## Taxonomic classification
> Coming soon!

## Functional annotation
> Coming soon!
