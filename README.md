## Download the GTDB files

First we download the tree files (Newick and taxonomic info)

```bash
wget https://data.ace.uq.edu.au/public/gtdb/data/releases/latest/bac120.tree
wget https://data.ace.uq.edu.au/public/gtdb/data/releases/latest/bac120_taxonomy.tsv
```

And we reformat the data from the taxonomy table so we have the different taxonomic leves in different columns (d=domain, p=phylum, c=clasm o=order, f=family, g=genus, s=species)

```bash
(echo -e "name\tDomain\tPhylum\tClass\tOrder\tFamily\tGenus\tSpecies"; awk -F'\t' '{split($2,a,";"); printf "%s", $1; for (i in a) { printf "\t%s", substr(a[i], 4) }; printf "\n"}' bac120_taxonomy.tsv) > bac120_taxonomy_taxonium.tsv
```


## Transform the data into Taxonium JSON format

Then we install taxonium tools if not previously installed

```bash
pip install taxoniumtools
```

Then we run taxonium tools to format everything into a single JSON:

```bash
newick_to_taxonium -i bac120.tree -m bac120_taxonomy_taxonium.tsv -o gtdb_taxonium.jsonl --key_column "name" -c Domain,Phylum,Class,Order,Family,Genus,Species
```

Add the file to taxonium using the protourl parameter and color by phylum 

```bash
https://taxonium.org/?protoUrl=https%3A%2F%2Fraw.githubusercontent.com%2Fpentamorfico%2Fgtdb_taxonium%2Frefs%2Fheads%2Fmain%2Fgtdb_taxonium.jsonl&xType=x_dist&color=%7B%22field%22%3A%22meta_Phylum%22%7D
```
