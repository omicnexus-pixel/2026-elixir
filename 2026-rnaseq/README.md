# Humán 22. kromoszóma RNA-Seq variancia-analízis

Ez a projekt a humán 22. kromoszóma RNA-Seq feldolgozási pipelineját tartalmazza: indexelés (HiSat2), illesztés, számlálás (featureCounts), és differenciális kifejeződés-elemzés (edgeR) PCA és hőtérképes vizualizációval.

## Követelmények
- **Pixi** (szükséges a függőségek kezeléséhez)

## Gyors indítás
A teljes pipeline futtatása:
```bash
make download  # Adatok letöltése
make index     # Referencia indexelése
make align SAMPLE=HBR_1  # Illesztés
make count     # Számlálás
make edger     # Differenciális kifejeződés-elemzés
make pca       # PCA vizualizáció
make heatmap   # Hőtérképa
```

Vagy az összes lépés egyszerre:
```bash
make all
```

## Kimeneti fájlok
- `bam/` — illesztési fájlok (BAM)
- `csv/` — számlálási és elemzési adatok (counts.csv, gene_expression.csv)
- `csv/pca.pdf` — PCA plot
- `csv/heatmap.pdf` — hőtérképa

## Megjegyzések
- A `featureCounts` reverse-stranded konfigurációval fut (`-s 2`).
- A további konfigurációkért lásd a `Makefile`-t és a `src/run/hisat2.mk` fájlt.



