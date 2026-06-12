# The URL to the genome.
FASTA_URL=https://ftp.ncbi.nlm.nih.gov/genomes/all/GCF/002/145/825/GCF_002145825.3_ASM214582v3/GCF_002145825.3_ASM214582v3_genomic.fna.gz

# The URL to the annotation.
GFF_URL=https://ftp.ncbi.nlm.nih.gov/genomes/all/GCF/002/145/825/GCF_002145825.3_ASM214582v3/GCF_002145825.3_ASM214582v3_genomic.gff.gz 

# Download the genome 
curl $FASTA_URL > hanta_Maporal.fna.gz 

# Download the annotation.
curl $GFF_URL > hanta_Maporal.gff.gz 

# Unzip the annotation.
gunzip hanta_Maporal.gff.gz 

# Unzip the genome.
gunzip hanta_Maporal.fna.gz 