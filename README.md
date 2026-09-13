# KEGG

```bash
mkdir -p kegg_ids
grep -o "KEGG:K....." genomes/Arthrobacter_cavernae_PO-11.gff3 | tr ":" "\t" > kegg_ids/Arthrobacter_cavernae_PO-11.gff3_kegg_ids.txt 
grep -o "KEGG:K....." genomes/M9409_L16.gff3 | tr ":" "\t" > kegg_ids/M9409_L16.gff3.gff3_kegg_ids.txt
```

## Kegg mapper colors
(https://www.genome.jp/kegg/mapper/color.html)
```bash
awk '{print $2 "\tblue"}' kegg_ids/Arthrobacter_cavernae_PO-11.gff3_kegg_ids.txt> kegg_ids/Arthrobacter_cavernae_PO-11.gff3_kegg_ids_color.txt
awk '{print $2 "\tred"}' kegg_ids/M9409_L16.gff3.gff3_kegg_ids.txt>kegg_ids/M9409_L16.gff3.gff3_kegg_ids_color.txt

cat kegg_ids/Arthrobacter_cavernae_PO-11.gff3_kegg_ids_color.txt kegg_ids/M9409_L16.gff3.gff3_kegg_ids_color.txt> kegg_ids/kegg_mapper_color.txt
```
submit file  `kegg_mapper_color.txt` to (https://www.genome.jp/kegg/mapper/color.html)

![](/home/julibote/github/genomics/pictures/kegg_mapper.png)

# MACSYFINDER
(https://github.com/gem-pasteur/macsyfinder)
Identify bacterial secretion systems

```bash
conda install -c bioconda macsyfinder
download the database size (36M)
macsydata install --target /home/julibote/TXSScan_model TXSScan -f
```

```bash
mkdir -p macsyfinder
ls proteins_fasta/*.faa > files_list.txt
while IFS= read -r fasta; do
    sample=$(basename "$fasta" .faa)
    macsyfinder --db-type ordered_replicon \
  --sequence-db "$fasta" \
  --models-dir /home/julibote/TXSScan_model \
  --models TXSScan all \
  -o macsyfinder/"$sample"
done < files_list.txt
```
check file `macsyfinder/Arthrobacter_cavernae_PO-11/best_solution_summary.tsv` 



