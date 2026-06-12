# Ebola vírus elemzés

Ez a projekt a Zaire ebolavírus (Mayinga) genomjának letöltését, indexelését, olvasatainak letöltését és illesztését, valamint variánsok hívását automatizálja.

## Mi található a `Makefile`-ben
A `Makefile` a következő fő lépéseket végzi el:
- `download`: genom FASTA és GFF fájl letöltése, kicsomagolása és BWA index készítése
- `fastq`: SRA olvasatok letöltése `fastq-dump` segítségével, limit `100000` végponttal
- `align`: BWA MEM alapú illesztés, BAM fájl készítése, rendezés és indexelés
- `vcf`: `bcftools` mpileup, hívás, normalizálás, rendezés, indexelés és VCF kicsomagolás
- `all`: a teljes folyamat futtatása sorrendben (`download`, `fastq`, `align`, `vcf`)

## Adatforrások
- Genom accession: `GCF_000848505.1`
- Genom FASTA: `https://ftp.ncbi.nlm.nih.gov/genomes/all/GCF/000/848/505/GCF_000848505.1_ViralProj14703/GCF_000848505.1_ViralProj14703_genomic.fna.gz`
- Genom GFF: `https://ftp.ncbi.nlm.nih.gov/genomes/all/GCF/000/848/505/GCF_000848505.1_ViralProj14703/GCF_000848505.1_ViralProj14703_genomic.gff.gz`
- SRA mintavétel: `SRR1553425`
- Bioproject: `PRJNA257197`

## Követelmények
A folyamat futtatásához a következő eszközök szükségesek:
- `wget`
- `gunzip`
- `bwa`
- `samtools`
- `fastq-dump` (SRA Toolkit)
- `bcftools`

## Használat
A `Makefile` használata előtt győződj meg róla, hogy az összes szükséges eszköz telepítve van.

A teljes munkafolyamat futtatása:
```sh
make all
```

Ha csak egy adott lépést szeretnél futtatni:
- `make download`
- `make fastq`
- `make align`
- `make vcf`

## Kimeneti fájlok
- `refs/ebola-mayinga.fasta`
- `refs/ebola-mayinga.gff`
- `reads/SRR1553425_1.fastq`
- `reads/SRR1553425_2.fastq`
- `bam/SRR1553425.bam.sorted.bam`
- `bam/SRR1553425.bam.sorted.bam.bai`
- `vcf/SRR1553425.vcf.gz`
- `vcf/SRR1553425.vcf.gz.tbi`

## Megjegyzés
A `Makefile` automatikusan létrehozza a szükséges könyvtárakat (`refs`, `reads`, `bam`, `vcf`). A `vcf` cél kimeneteként létrejön és indexelődik a `vcf/SRR1553425.vcf.gz`, majd a fájl kicsomagolásra kerül.
