$ # # The URL to the genome.
FASTA_URL=https://ftp.ncbi.nlm.nih.gov/genomes/all/GCF/002/366/285/GCF_002366285.1_ViralProj411812/GCF_002366285.1_ViralProj411812_genomic.fna.gz
# The URL to the annotation.
GFF_URL=https://ftp.ncbi.nlm.nih.gov/genomes/all/GCF/002/366/285/GCF_002366285.1_ViralProj411812/GCF_002366285.1_ViralProj411812_genomic.gff.gz

# Download the genome
curl $FASTA_URL > Zika_Natal.fna.gz

# Download the annotation.
curl $GFF_URL > Zika_Natal.gff.gz

# Unzip the annotation.
gunzip Zika_Natal.gff.gz

# Unzip the genome
gunzip Zika_Natal.fna.gz