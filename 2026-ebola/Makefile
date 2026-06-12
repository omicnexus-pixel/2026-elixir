FASTA_URL=https://ftp.ncbi.nlm.nih.gov/genomes/all/GCF/000/848/505/GCF_000848505.1_ViralProj14703/GCF_000848505.1_ViralProj14703_genomic.fna.gz

# The URL to the annotation.
GFF_URL=https://ftp.ncbi.nlm.nih.gov/genomes/all/GCF/000/848/505/GCF_000848505.1_ViralProj14703/GCF_000848505.1_ViralProj14703_genomic.gff.gz

# The name of the genome.
GENOME_NAME=ebola-mayinga

# The path to the genome FASTA file.
GENOME_FASTA=refs/${GENOME_NAME}.fasta

# The path to the genome GFF file.
GENOME_GFF=refs/${GENOME_NAME}.gff

# The SRR ID of the sample.
SRR=SRR1553425

# The name of the alignment BAM file
BAM=bam/${SRR}.bam

# The variant file
VCF=vcf/${SRR}.vcf.gz

# Additional bcf flags for pileup annotation.
PILE_FLAGS =  -d 100 --annotate 'INFO/AD,FORMAT/DP,FORMAT/AD,FORMAT/ADF,FORMAT/ADR,FORMAT/SP'

# Additional bcf flags for calling process.
CALL_FLAGS = --ploidy 2 --annotate 'FORMAT/GQ'

# The number of reads to download.
LIMIT=100000

# The path to the read files
R1_READS=reads/${SRR}_1.fastq
R2_READS=reads/${SRR}_2.fastq

# ----- No CHANGES BELOW THIS LINE 

help:
	@echo "Use the source, Luke!"

# make the directories that are needed then download and unzip the fasta and gff files
download:
	mkdir -p refs
	wget -O ${GENOME_FASTA}.gz ${FASTA_URL}
	wget -O ${GENOME_GFF}.gz ${GFF_URL}
	gunzip -f ${GENOME_FASTA}.gz
	gunzip -f ${GENOME_GFF}.gz
	bwa index ${GENOME_FASTA}

# Download the reads from SRA, limit to LIMIT reads use the fastq-dump command
fastq:
	mkdir -p reads
	fastq-dump -X ${LIMIT} --outdir reads --split-files ${SRR}

# Align the reads to the genome
align:
	mkdir -p bam
	bwa mem -t 4 ${GENOME_FASTA} ${R1_READS} ${R2_READS} | samtools view -bS - > ${BAM}
	samtools sort ${BAM} -o ${BAM}.sorted.bam
	samtools index ${BAM}.sorted.bam

# Make vcf file with bcftools mpileup and call, then sort and index the vcf file 
vcf:
	mkdir -p vcf
	bcftools mpileup ${PILE_FLAGS} -O u -f ${GENOME_FASTA} ${BAM}.sorted.bam | \
	bcftools call ${CALL_FLAGS} -mv -O u | \
	bcftools norm -f ${GENOME_FASTA} -d all -O u | \
	bcftools sort -O z > ${VCF}
	bcftools index -t -f ${VCF}
	gunzip -f ${VCF}

all: download fastq align vcf

.PHONY: all help download fastq align vcf




