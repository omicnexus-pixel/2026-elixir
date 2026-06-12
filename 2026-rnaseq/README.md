# RNA-Seq elemzés (HiSat2 alapú pipeline)

Ez a repository egy egyszerű RNA-Seq feldolgozó pipeline-t tartalmaz, amely a `Makefile` céljaira épül. A célok automatizálják az adatok letöltését, indexelését, illesztését, számlálását és alapvető differenciális kifejeződés-elemzéseket (edgeR), valamint egyszerű ábrázolásokat (PCA, heatmap).

## Követelmények
- `make`
- `curl`, `tar`
- `hisat2` (indexelés/illesztés a `src/run/hisat2.mk` használatával)
- `featureCounts` (subread csomag)
- `R` + szükséges R-scriptek és csomagok (a `src/r/` mappában található scriptek használata)

## Gyors áttekintés
- Referencia genomi fájl: `refs/chr22.genome.fa`
- Anotáció (GTF): `refs/chr22.gtf`
- Minták és olvasások a `reads/` mappában (`HBR_*`, `UHR_*`)
- Kimenetek a `bam/` és `csv/` mappákba kerülnek

## Fontos Make célok
- `make help` — Kiírja a fontos változókat és célokat.
- `make download` — Letölti és kicsomagolja a bemeneti adatokat a `DATA_URL` alapján.
- `make index` — Referencia indexelése (`src/run/hisat2.mk` használatával).
- `make align` — Illeszti a `SAMPLE`-ként megadott mintát a referenciához.
- `make count` — Számlálja az illesztéseket `featureCounts`-szal (jelenleg a `-s 2` opcióval, azaz reverse-stranded).
- `make to_csv` — Átalakítja a `featureCounts` kimenetet CSV formátumba (`src/r/format_featurecounts.r`).
- `make add_names` — Hozzáadja a génneveket a számlálási fájlhoz (a `tx2gene` létrehozása után).
- `make edger` — Futtatja az `edgeR` elemzést (`src/r/edger.r`) a `design.csv` és a `csv/counts.csv` alapján; kimenet: `csv/gene_expression.csv`.
- `make pca` — Elkészíti a PCA ábrát (`csv/pca.pdf`).
- `make heatmap` — Elkészíti a hőtérképet (`csv/heatmap.pdf`).
- `make design` — Kiírja/megmutatja a `design.csv` fájlt.

## Használati példák
Példák parancsokra a projekt gyökeréből:

```bash
make download
make index
make align SAMPLE=HBR_1
make count
make to_csv
make edger
make pca
make heatmap
```

## Megjegyzések
- A `count` cél a Makefile-ban konkrét `bam/*` fájlokat sorol fel; ha új mintákat adsz hozzá, frissítsd a cél sort vagy biztosítsd, hogy a `bam/` könyvtár tartalmazza a megfelelő `.bam` fájlokat.
- A `featureCounts` opciója `-s 2`, tehát a bemeneti könyvtár reverse-stranded könyvtárra van konfigurálva. Szükség esetén módosítsd ezt a megfelelő értékre.
- A `align` és `index` célok a `src/run/hisat2.mk` makefile-t használják; további konfigurációkért nézd meg azt a fájlt.


