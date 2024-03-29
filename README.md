# SCN_metagenome_code
Workflow for SCN Illumina NovaSeq metagenomes

fastqc *.fastq.gz -t 40

#### Received new fastq files of SR and SS for the "mid" samples. I need to concat the two sets together. 
cat SR_S14_L002_R1_001.fastq.gz SR-LTRMidCysts-3-6-23_S2_L003_R1_001.fastq.gz > SR_Mid_R1_001.fastq.gz

cat SR_S14_L002_R2_001.fastq.gz SR-LTRMidCysts-3-6-23_S2_L003_R2_001.fastq.gz > SR_Mid_R2_001.fastq.gz

cat SS_S12_L002_R1_001.fastq.gz SS-LTRMidCysts-3-6-23_S1_L003_R1_001.fastq.gz > SS_Mid_R1_001.fastq.gz

cat SS_S12_L002_R2_001.fastq.gz SS-LTRMidCysts-3-6-23_S1_L003_R2_001.fastq.gz > SS_Mid_R2_001.fastq.gz

fastqc SR_Mid* -t 40
fastqc SS_Mid* -t 40

#### The sequence lengths are variable (35-151) and the Per base sequence content of the frist 14ish basepairs are not good.  
#### First, only include reads that are >150 bp
#### CGI prepared with NexteraXT, remove nextera adapters, then remove the ~16bp from the reads. the "pt" means paired & trimmed.   

#### Remove adapters and then trim to even length
java -jar /programs/trimmomatic/trimmomatic-0.39.jar PE -threads 20 -phred33 S3_S11_L002_R1_001.fastq.gz S3_S11_L002_R2_001.fastq.gz S3cystsmid_p_R1.fastq.gz S3cystsmid_u_R1.fastq.gz S3cystsmid_p_R2.fastq.gz S3cystsmid_u_R2.fastq.gz ILLUMINACLIP:/programs/trimmomatic/adapters/NexteraPE-PE.fa:2:30:10 LEADING:3 TRAILING:3 SLIDINGWINDOW:4:15 MINLEN:150

java -jar /programs/trimmomatic/trimmomatic-0.39.jar PE -threads 40 -phred33 SA_S13_L002_R1_001.fastq.gz SA_S13_L002_R2_001.fastq.gz SAcystsmid_p_R1.fastq.gz SAcystsmid_u_R1.fastq.gz SAcystsmid_p_R2.fastq.gz SAcystsmid_u_R2.fastq.gz ILLUMINACLIP:/programs/trimmomatic/adapters/NexteraPE-PE.fa:2:30:10 LEADING:3 TRAILING:3 SLIDINGWINDOW:4:15 MINLEN:150

java -jar /programs/trimmomatic/trimmomatic-0.39.jar PE -threads 40 -phred33 SR_Mid_R1_001.fastq.gz SR_Mid_R2_001.fastq.gz SRcystsmid_p_R1.fastq.gz SRcystsmid_u_R1.fastq.gz SRcystsmid_p_R2.fastq.gz SRcystsmid_u_R2.fastq.gz ILLUMINACLIP:/programs/trimmomatic/adapters/NexteraPE-PE.fa:2:30:10 LEADING:3 TRAILING:3 SLIDINGWINDOW:4:15 MINLEN:150

java -jar /programs/trimmomatic/trimmomatic-0.39.jar PE -threads 40 -phred33 SS_Mid_R1_001.fastq.gz SS_Mid_R2_001.fastq.gz SScystsmid_p_R1.fastq.gz SScystsmid_u_R1.fastq.gz SScystsmid_p_R2.fastq.gz SScystsmid_u_R2.fastq.gz ILLUMINACLIP:/programs/trimmomatic/adapters/NexteraPE-PE.fa:2:30:10 LEADING:3 TRAILING:3 SLIDINGWINDOW:4:15 MINLEN:150

java -jar /programs/trimmomatic/trimmomatic-0.39.jar PE -threads 40 -phred33 S3-LTR-Fall-Cysts-5-31-23_S6_L003_R1_001.fastq.gz S3-LTR-Fall-Cysts-5-31-23_S6_L003_R2_001.fastq.gz S3cystsfall_p_R1.fastq.gz S3cystsfall_u_R1.fastq.gz S3cystsfall_p_R2.fastq.gz S3cystsfall_u_R2.fastq.gz ILLUMINACLIP:/programs/trimmomatic/adapters/NexteraPE-PE.fa:2:30:10 LEADING:3 TRAILING:3 SLIDINGWINDOW:4:15 MINLEN:150

java -jar /programs/trimmomatic/trimmomatic-0.39.jar PE -threads 40 -phred33 SA-LTR-Fall-Cysts-5-31-23_S5_L003_R1_001.fastq.gz SA-LTR-Fall-Cysts-5-31-23_S5_L003_R2_001.fastq.gz SAcystsfall_p_R1.fastq.gz SAcystsfall_u_R1.fastq.gz SAcystsfall_p_R2.fastq.gz SAcystsfall_u_R2.fastq.gz ILLUMINACLIP:/programs/trimmomatic/adapters/NexteraPE-PE.fa:2:30:10 LEADING:3 TRAILING:3 SLIDINGWINDOW:4:15 MINLEN:150

java -jar /programs/trimmomatic/trimmomatic-0.39.jar PE -threads 40 -phred33 SR-LTR-Fall-Cysts-5-31-23_S3_L003_R1_001.fastq.gz SR-LTR-Fall-Cysts-5-31-23_S3_L003_R2_001.fastq.gz SRcystsfall_p_R1.fastq.gz SRcystsfall_u_R1.fastq.gz SRcystsfall_p_R2.fastq.gz SRcystsfall_u_R2.fastq.gz ILLUMINACLIP:/programs/trimmomatic/adapters/NexteraPE-PE.fa:2:30:10 LEADING:3 TRAILING:3 SLIDINGWINDOW:4:15 MINLEN:150

java -jar /programs/trimmomatic/trimmomatic-0.39.jar PE -threads 40 -phred33 SS-LTR-Fall-Cysts-5-31-23_S4_L003_R1_001.fastq.gz SS-LTR-Fall-Cysts-5-31-23_S4_L003_R2_001.fastq.gz SScystsfall_p_R1.fastq.gz SScystsfall_u_R1.fastq.gz SScystsfall_p_R2.fastq.gz SScystsfall_u_R2.fastq.gz ILLUMINACLIP:/programs/trimmomatic/adapters/NexteraPE-PE.fa:2:30:10 LEADING:3 TRAILING:3 SLIDINGWINDOW:4:15 MINLEN:150


#### remove the ~16bp from the reads
java -jar /programs/trimmomatic/trimmomatic-0.39.jar SE -threads 40 -phred33 S3cystsmid_p_R1.fastq.gz S3cystsmid_pt_R1.fastq.gz HEADCROP:16

java -jar /programs/trimmomatic/trimmomatic-0.39.jar SE -threads 40 -phred33 S3cystsmid_p_R2.fastq.gz S3cystsmid_pt_R2.fastq.gz HEADCROP:16

java -jar /programs/trimmomatic/trimmomatic-0.39.jar SE -threads 40 -phred33 SAcystsmid_p_R1.fastq.gz SAcystsmid_pt_R1.fastq.gz HEADCROP:16

java -jar /programs/trimmomatic/trimmomatic-0.39.jar SE -threads 40 -phred33 SAcystsmid_p_R2.fastq.gz SAcystsmid_pt_R2.fastq.gz HEADCROP:16

java -jar /programs/trimmomatic/trimmomatic-0.39.jar SE -threads 40 -phred33 SRcystsmid_p_R1.fastq.gz SRcystsmid_pt_R1.fastq.gz HEADCROP:16

java -jar /programs/trimmomatic/trimmomatic-0.39.jar SE -threads 40 -phred33 SRcystsmid_p_R2.fastq.gz SRcystsmid_pt_R2.fastq.gz HEADCROP:16

java -jar /programs/trimmomatic/trimmomatic-0.39.jar SE -threads 40 -phred33 SScystsmid_p_R1.fastq.gz SScystsmid_pt_R1.fastq.gz HEADCROP:16

java -jar /programs/trimmomatic/trimmomatic-0.39.jar SE -threads 40 -phred33 SScystsmid_p_R2.fastq.gz SScystsmid_pt_R2.fastq.gz HEADCROP:16

java -jar /programs/trimmomatic/trimmomatic-0.39.jar SE -threads 40 -phred33 S3cystsfall_p_R1.fastq.gz S3cystsfall_pt_R1.fastq.gz HEADCROP:16

java -jar /programs/trimmomatic/trimmomatic-0.39.jar SE -threads 40 -phred33 S3cystsfall_p_R2.fastq.gz S3cystsfall_pt_R2.fastq.gz HEADCROP:16

java -jar /programs/trimmomatic/trimmomatic-0.39.jar SE -threads 40 -phred33 SAcystsfall_p_R1.fastq.gz SAcystsfall_pt_R1.fastq.gz HEADCROP:16

java -jar /programs/trimmomatic/trimmomatic-0.39.jar SE -threads 40 -phred33 SAcystsfall_p_R2.fastq.gz SAcystsfall_pt_R2.fastq.gz HEADCROP:16

java -jar /programs/trimmomatic/trimmomatic-0.39.jar SE -threads 40 -phred33 SRcystsfall_p_R1.fastq.gz SRcystsfall_pt_R1.fastq.gz HEADCROP:16

java -jar /programs/trimmomatic/trimmomatic-0.39.jar SE -threads 40 -phred33 SRcystsfall_p_R2.fastq.gz SRcystsfall_pt_R2.fastq.gz HEADCROP:16

java -jar /programs/trimmomatic/trimmomatic-0.39.jar SE -threads 40 -phred33 SScystsfall_p_R1.fastq.gz SScystsfall_pt_R1.fastq.gz HEADCROP:16

java -jar /programs/trimmomatic/trimmomatic-0.39.jar SE -threads 40 -phred33 SScystsfall_p_R2.fastq.gz SScystsfall_pt_R2.fastq.gz HEADCROP:16

### run fastqc 
fastqc *_pt_R1.fastq.gz

fastqc *_pt_R2.fastq.gz



#### Use bowtie to remove both the SCN and soybean reads from the raw files. 
#### Found this website: https://www.metagenomics.wiki/tools/short-read/remove-host-sequences & https://paleogenomics-course.readthedocs.io/en/latest/4_ReadsMapping_v2.html
#### Reference Nematode genome: GenBank assembly accession:GCA_004148225.2 (latest), https://www.ncbi.nlm.nih.gov/assembly/GCA_004148225.2
#### Soybean genome used: https://www.ncbi.nlm.nih.gov/assembly/GCA_022114995.1

#### concat files together for a database: https://www.metagenomics.wiki/tools/bowtie2/index
cat GCA_022114995.1_ASM2211499v1_genomic.fna GCA_004148225.2_ASM414822v2_genomic.fna > scn_sb_genomes.fna

A) bowtie2 mapping against host sequence, create bowtie2 database using a reference genome (fasta file)
/programs/bowtie2-2.4.5-linux-x86_64/bowtie2-build scn_sb_genomes.fna scn_sb_DB

#### bowtie2 mapping against host sequence database, keep both aligned and unaligned reads (paired-end reads)
/programs/bowtie2-2.4.5-linux-x86_64/bowtie2 -p 12 -x scn_sb_DB -1 S3cystsmid_pt_R1.fastq.gz -2 S3cystsmid_pt_R2.fastq.gz -S S3cystsmid_mapped_and_unmapped.sam

/programs/bowtie2-2.4.5-linux-x86_64/bowtie2 -p 12 -x scn_sb_DB -1 SAcystsmid_pt_R1.fastq.gz -2 SAcystsmid_pt_R2.fastq.gz -S SAcystsmid_mapped_and_unmapped.sam

/programs/bowtie2-2.4.5-linux-x86_64/bowtie2 -p 12 -x scn_sb_DB -1 SRcystsmid_pt_R1.fastq.gz -2 SRcystsmid_pt_R2.fastq.gz -S SRcystsmid_mapped_and_unmapped.sam

/programs/bowtie2-2.4.5-linux-x86_64/bowtie2 -p 12 -x scn_sb_DB -1 SScystsmid_pt_R1.fastq.gz -2 SScystsmid_pt_R2.fastq.gz -S SScystsmid_mapped_and_unmapped.sam

/programs/bowtie2-2.4.5-linux-x86_64/bowtie2 -p 12 -x scn_sb_DB -1 S3cystsfall_pt_R1.fastq.gz -2 S3cystsfall_pt_R2.fastq.gz -S S3cystsfall_mapped_and_unmapped.sam

/programs/bowtie2-2.4.5-linux-x86_64/bowtie2 -p 12 -x scn_sb_DB -1 SAcystsfall_pt_R1.fastq.gz -2 SAcystsfall_pt_R2.fastq.gz -S SAcystsfall_mapped_and_unmapped.sam

/programs/bowtie2-2.4.5-linux-x86_64/bowtie2 -p 12 -x scn_sb_DB -1 SRcystsfall_pt_R1.fastq.gz -2 SRcystsfall_pt_R2.fastq.gz -S SRcystsfall_mapped_and_unmapped.sam

/programs/bowtie2-2.4.5-linux-x86_64/bowtie2 -p 12 -x scn_sb_DB -1 SScystsfall_pt_R1.fastq.gz -2 SScystsfall_pt_R2.fastq.gz -S SScystsfall_mapped_and_unmapped.sam
#OUT PUT OF OVERALL ALIGNMENT RATE

#### convert file .sam to .bam
/programs/samtools-1.15.1-r/bin/samtools view -bS S3cystsmid_mapped_and_unmapped.sam > S3cystsmid_mapped_and_unmapped.bam

/programs/samtools-1.15.1-r/bin/samtools view -bS SAcystsmid_mapped_and_unmapped.sam > SAcystsmid_mapped_and_unmapped.bam

/programs/samtools-1.15.1-r/bin/samtools view -bS SRcystsmid_mapped_and_unmapped.sam > SRcystsmid_mapped_and_unmapped.bam

/programs/samtools-1.15.1-r/bin/samtools view -bS SScystsmid_mapped_and_unmapped.sam > SScystsmid_mapped_and_unmapped.bam

/programs/samtools-1.15.1-r/bin/samtools view -bS S3cystsfall_mapped_and_unmapped.sam > S3cystsfall_mapped_and_unmapped.bam

/programs/samtools-1.15.1-r/bin/samtools view -bS SAcystsfall_mapped_and_unmapped.sam > SAcystsfall_mapped_and_unmapped.bam

/programs/samtools-1.15.1-r/bin/samtools view -bS SRcystsfall_mapped_and_unmapped.sam > SRcystsfall_mapped_and_unmapped.bam

/programs/samtools-1.15.1-r/bin/samtools view -bS SScystsfall_mapped_and_unmapped.sam > SScystsfall_mapped_and_unmapped.bam

#B) Filter required unmapped reads
#### SAMtools SAM-flag filter: get unmapped pairs (both reads R1 and R2 unmapped)
/programs/samtools-1.15.1-r/bin/samtools view -b -f 12 -F 256 S3cystsmid_mapped_and_unmapped.bam > S3cystsmid_bothReadsUnmapped.bam

/programs/samtools-1.15.1-r/bin/samtools view -b -f 12 -F 256 SAcystsmid_mapped_and_unmapped.bam > SAcystsmid_bothReadsUnmapped.bam

/programs/samtools-1.15.1-r/bin/samtools view -b -f 12 -F 256 SRcystsmid_mapped_and_unmapped.bam > SRcystsmid_bothReadsUnmapped.bam

/programs/samtools-1.15.1-r/bin/samtools view -b -f 12 -F 256 SScystsmid_mapped_and_unmapped.bam > SScystsmid_bothReadsUnmapped.bam

/programs/samtools-1.15.1-r/bin/samtools view -b -f 12 -F 256 S3cystsfall_mapped_and_unmapped.bam > S3cystsfall_bothReadsUnmapped.bam

/programs/samtools-1.15.1-r/bin/samtools view -b -f 12 -F 256 SAcystsfall_mapped_and_unmapped.bam > SAcystsfall_bothReadsUnmapped.bam

/programs/samtools-1.15.1-r/bin/samtools view -b -f 12 -F 256 SRcystsfall_mapped_and_unmapped.bam > SRcystsfall_bothReadsUnmapped.bam

/programs/samtools-1.15.1-r/bin/samtools view -b -f 12 -F 256 SScystsfall_mapped_and_unmapped.bam > SScystsfall_bothReadsUnmapped.bam

#### sort bam file by read name ( -n ) to have paired reads next to each other (8 parallel threads, each using up to 5G memory)
#C) split paired-end reads into separated fastq files.
/programs/samtools-1.15.1-r/bin/samtools sort -n -m 5G -@ 12 S3cystsmid_bothReadsUnmapped.bam -o S3cystsmid_bothReadsUnmapped_sorted.bam

/programs/samtools-1.15.1-r/bin/samtools fastq -@ 12 S3cystsmid_bothReadsUnmapped_sorted.bam -1 S3cystsmid_R1.fastq.gz -2 S3cystsmid_R2.fastq.gz -0 /dev/null -s /dev/null -n

/programs/samtools-1.15.1-r/bin/samtools sort -n -m 5G -@ 12 SAcystsmid_bothReadsUnmapped.bam -o SAcystsmid_bothReadsUnmapped_sorted.bam

/programs/samtools-1.15.1-r/bin/samtools fastq -@ 12 SAcystsmid_bothReadsUnmapped_sorted.bam -1 SAcystsmid_R1.fastq.gz -2 SAcystsmid_R2.fastq.gz -0 /dev/null -s /dev/null -n

/programs/samtools-1.15.1-r/bin/samtools sort -n -m 5G -@ 12 SRcystsmid_bothReadsUnmapped.bam -o SRcystsmid_bothReadsUnmapped_sorted.bam

/programs/samtools-1.15.1-r/bin/samtools fastq -@ 12 SRcystsmid_bothReadsUnmapped_sorted.bam -1 SRcystsmid_R1.fastq.gz -2 SRcystsmid_R2.fastq.gz -0 /dev/null -s /dev/null -n

/programs/samtools-1.15.1-r/bin/samtools sort -n -m 5G -@ 12 SScystsmid_bothReadsUnmapped.bam -o SScystsmid_bothReadsUnmapped_sorted.bam

/programs/samtools-1.15.1-r/bin/samtools fastq -@ 12 SScystsmid_bothReadsUnmapped_sorted.bam -1 SScystsmid_R1.fastq.gz -2 SScystsmid_R2.fastq.gz -0 /dev/null -s /dev/null -n

/programs/samtools-1.15.1-r/bin/samtools sort -n -m 5G -@ 12 S3cystsfall_bothReadsUnmapped.bam -o S3cystsfall_bothReadsUnmapped_sorted.bam

/programs/samtools-1.15.1-r/bin/samtools fastq -@ 12 S3cystsfall_bothReadsUnmapped_sorted.bam -1 S3cystsfall_R1.fastq.gz -2 S3cystsfall_R2.fastq.gz -0 /dev/null -s /dev/null -n

/programs/samtools-1.15.1-r/bin/samtools sort -n -m 5G -@ 12 SAcystsfall_bothReadsUnmapped.bam -o SAcystsfall_bothReadsUnmapped_sorted.bam

/programs/samtools-1.15.1-r/bin/samtools fastq -@ 12 SAcystsfall_bothReadsUnmapped_sorted.bam -1 SAcystsfall_R1.fastq.gz -2 SAcystsfall_R2.fastq.gz -0 /dev/null -s /dev/null -n

/programs/samtools-1.15.1-r/bin/samtools sort -n -m 5G -@ 12 SRcystsfall_bothReadsUnmapped.bam -o SRcystsfall_bothReadsUnmapped_sorted.bam

/programs/samtools-1.15.1-r/bin/samtools fastq -@ 12 SRcystsfall_bothReadsUnmapped_sorted.bam -1 SRcystsfall_R1.fastq.gz -2 SRcystsfall_R2.fastq.gz -0 /dev/null -s /dev/null -n

/programs/samtools-1.15.1-r/bin/samtools sort -n -m 5G -@ 8 SScystsfall_bothReadsUnmapped.bam -o SScystsfall_bothReadsUnmapped_sorted.bam

/programs/samtools-1.15.1-r/bin/samtools fastq -@ 8 SScystsfall_bothReadsUnmapped_sorted.bam -1 SScystsfall_R1.fastq.gz -2 SScystsfall_R2.fastq.gz -0 /dev/null -s /dev/null -n


### Assemble with megahit, Below was put in to bash script. megahitassembly.sh
#!/bin/bash
/programs/MEGAHIT-1.2.9-Linux-x86_64-static/bin/megahit -t 40 -1 S3cystsmid_pt_R1.fastq.gz -2 S3cystsmid_pt_R2.fastq.gz -o S3_Midassembly --out-prefix S3_mid

/programs/MEGAHIT-1.2.9-Linux-x86_64-static/bin/megahit -t 40 -1 SAcystsmid_pt_R1.fastq.gz -2 SAcystsmid_pt_R2.fastq.gz -o SA_Midassembly --out-prefix SA_mid

/programs/MEGAHIT-1.2.9-Linux-x86_64-static/bin/megahit -t 40 -1 SRcystsmid_pt_R1.fastq.gz -2 SRcystsmid_pt_R2.fastq.gz -o SR_Midassembly --out-prefix SR_mid

/programs/MEGAHIT-1.2.9-Linux-x86_64-static/bin/megahit -t 40 -1 SScystsmid_pt_R1.fastq.gz -2 SScystsmid_pt_R2.fastq.gz -o SS_Midassembly --out-prefix SS_mid

/programs/MEGAHIT-1.2.9-Linux-x86_64-static/bin/megahit -t 40 -1 S3cystsfall_pt_R1.fastq.gz -2 S3cystsfall_pt_R2.fastq.gz -o S3_Fallassembly --out-prefix S3_fall

/programs/MEGAHIT-1.2.9-Linux-x86_64-static/bin/megahit -t 40 -1 SAcystsfall_pt_R1.fastq.gz -2 SAcystsfall_pt_R2.fastq.gz -o SA_Fallassembly --out-prefix SA_fall

/programs/MEGAHIT-1.2.9-Linux-x86_64-static/bin/megahit -t 40 -1 SRcystsfall_pt_R1.fastq.gz -2 SRcystsfall_pt_R2.fastq.gz -o SR_Fallassembly --out-prefix SR_fall

/programs/MEGAHIT-1.2.9-Linux-x86_64-static/bin/megahit -t 40 -1 SScystsfall_pt_R1.fastq.gz -2 SScystsfall_pt_R2.fastq.gz -o SS_Fallassembly --out-prefix SS_fall


### Quast on assembly
export PYTHONPATH=/programs/quast-5.2.0/lib64/python3.9/site-packages:/programs/quast-5.2.0/lib/python3.9/site-packages

export PATH=/programs/quast-5.2.0/bin:$PATH

quast.py S3_Midassembly/S3_mid.contigs.fa --threads 32 -o S3_Midassembly/S3_Mid_quast

quast.py SA_Midassembly/SA_mid.contigs.fa --threads 32 -o SA_Midassembly/SA_Mid_quast

quast.py SR_Midassembly/SR_mid.contigs.fa --threads 32 -o SR_Midassembly/SR_Mid_quast

quast.py SS_Midassembly/SS_mid.contigs.fa --threads 32 -o SS_Midassembly/SS_Mid_quast

quast.py S3_Fallassembly/S3_fall.contigs.fa --threads 32 -o S3_Fallassembly/S3_Fall_quast

quast.py SA_Fallassembly/SA_fall.contigs.fa --threads 32 -o SA_Fallassembly/SA_Fall_quast

quast.py SR_Fallassembly/SR_fall.contigs.fa --threads 32 -o SR_Fallassembly/SR_Fall_quast

quast.py SS_Fallassembly/SS_fall.contigs.fa --threads 32 -o SS_Fallassembly/SS_Fall_quast


### Filter contigs less than 2500
/programs/bbmap-39.01/reformat.sh in=S3_Midassembly/S3_mid.contigs.fa out=S3_Midassembly/S3_midfilt.contigs.fa minlength=2500

/programs/bbmap-39.01/reformat.sh in=SA_Midassembly/SA_mid.contigs.fa out=SA_Midassembly/SA_midfilt.contigs.fa minlength=2500

/programs/bbmap-39.01/reformat.sh in=SR_Midassembly/SR_mid.contigs.fa out=SR_Midassembly/SR_midfilt.contigs.fa minlength=2500

/programs/bbmap-39.01/reformat.sh in=SS_Midassembly/SS_mid.contigs.fa out=SS_Midassembly/SS_midfilt.contigs.fa minlength=2500

/programs/bbmap-39.01/reformat.sh in=S3_Fallassembly/S3_fall.contigs.fa out=S3_Fallassembly/S3_fallfilt.contigs.fa minlength=2500

/programs/bbmap-39.01/reformat.sh in=SA_Fallassembly/SA_fall.contigs.fa out=SA_Fallassembly/SA_fallfilt.contigs.fa minlength=2500

/programs/bbmap-39.01/reformat.sh in=SR_Fallassembly/SR_fall.contigs.fa out=SR_Fallassembly/SR_fallfilt.contigs.fa minlength=2500

/programs/bbmap-39.01/reformat.sh in=SS_Fallassembly/SS_fall.contigs.fa out=SS_Fallassembly/SS_fallfilt.contigs.fa minlength=2500


### Quast on filtered assembly 
quast.py S3_Midassembly/S3_midfilt.contigs.fa --threads 32 -o S3_Midassembly/S3_Mid_quastfilt

quast.py SA_Midassembly/SA_midfilt.contigs.fa --threads 32 -o SA_Midassembly/SA_Mid_quastfilt

quast.py SR_Midassembly/SR_midfilt.contigs.fa --threads 32 -o SR_Midassembly/SR_Mid_quastfilt

quast.py SS_Midassembly/SS_midfilt.contigs.fa --threads 32 -o SS_Midassembly/SS_Mid_quastfilt

quast.py S3_Fallassembly/S3_fallfilt.contigs.fa --threads 32 -o S3_Fallassembly/S3_Fall_quastfilt

quast.py SA_Fallassembly/SA_fallfilt.contigs.fa --threads 32 -o SA_Fallassembly/SA_Fall_quastfilt

quast.py SR_Fallassembly/SR_fallfilt.contigs.fa --threads 32 -o SR_Fallassembly/SR_Fall_quastfilt

quast.py SS_Fallassembly/SS_fallfilt.contigs.fa --threads 32 -o SS_Fallassembly/SS_Fall_quastfilt


### Blast filtered contigs against scn genome & soybean genome again 
cat GCA_022114995.1_ASM2211499v1_genomic.fna GCA_004148225.2_ASM414822v2_genomic.fna > soybeanscngenomes.fasta

#### make database
makeblastdb -in soybeanscngenomes.fasta -parse_seqids -dbtype nucl  -title SBSCNgenome -out SBSCNDB

/programs/ncbi-blast-2.9.0+/bin/blastn -db SBSCNDB -query S3_Midassembly/S3_midfilt.contigs.fa -out S3_Midassembly/S3_midfilt.results -outfmt "6 qseqid sseqid pident length qstart qend sstart send evalue bitscore staxids sscinames sskingdoms stitle" -max_target_seqs 3 -max_hsps 1 -num_threads 40

/programs/ncbi-blast-2.9.0+/bin/blastn -db SBSCNDB -query SA_Midassembly/SA_midfilt.contigs.fa -out SA_Midassembly/SA_midfilt.results -outfmt "6 qseqid sseqid pident length qstart qend sstart send evalue bitscore staxids sscinames sskingdoms stitle" -max_target_seqs 3 -max_hsps 1 -num_threads 40

/programs/ncbi-blast-2.9.0+/bin/blastn -db SBSCNDB -query SR_Midassembly/SR_midfilt.contigs.fa -out SR_Midassembly/SR_midfilt.results -outfmt "6 qseqid sseqid pident length qstart qend sstart send evalue bitscore staxids sscinames sskingdoms stitle" -max_target_seqs 3 -max_hsps 1 -num_threads 40

/programs/ncbi-blast-2.9.0+/bin/blastn -db SBSCNDB -query SS_Midassembly/SS_midfilt.contigs.fa -out SS_Midassembly/SS_midfilt.results -outfmt "6 qseqid sseqid pident length qstart qend sstart send evalue bitscore staxids sscinames sskingdoms stitle" -max_target_seqs 3 -max_hsps 1 -num_threads 40

/programs/ncbi-blast-2.9.0+/bin/blastn -db SBSCNDB -query S3_Fallassembly/S3_fallfilt.contigs.fa -out S3_Fallassembly/S3_fallfilt.results -outfmt "6 qseqid sseqid pident length qstart qend sstart send evalue bitscore staxids sscinames sskingdoms stitle" -max_target_seqs 3 -max_hsps 1 -num_threads 40

/programs/ncbi-blast-2.9.0+/bin/blastn -db SBSCNDB -query SA_Fallassembly/SA_fallfilt.contigs.fa -out SA_Fallassembly/SA_fallfilt.results -outfmt "6 qseqid sseqid pident length qstart qend sstart send evalue bitscore staxids sscinames sskingdoms stitle" -max_target_seqs 3 -max_hsps 1 -num_threads 40

/programs/ncbi-blast-2.9.0+/bin/blastn -db SBSCNDB -query SR_Fallassembly/SR_fallfilt.contigs.fa -out SR_Fallassembly/SR_fallfilt.results -outfmt "6 qseqid sseqid pident length qstart qend sstart send evalue bitscore staxids sscinames sskingdoms stitle" -max_target_seqs 3 -max_hsps 1 -num_threads 40

/programs/ncbi-blast-2.9.0+/bin/blastn -db SBSCNDB -query SS_Fallassembly/SS_fallfilt.contigs.fa -out SS_Fallassembly/SS_fallfilt.results -outfmt "6 qseqid sseqid pident length qstart qend sstart send evalue bitscore staxids sscinames sskingdoms stitle" -max_target_seqs 3 -max_hsps 1 -num_threads 40


#### Load the version of python! 3.10.6 then run code 
module load python/3.10.6

### Remove contigs that hit to soybean or scn genome
python filter_fasta_by_list_of_headers.py S3_Midassembly/S3_midfilt.contigs.fa S3_Midassembly/S3_mid_contigstoremove.txt > S3_Midassembly/S3_midfilt2.fa

python filter_fasta_by_list_of_headers.py SA_Midassembly/SA_midfilt.contigs.fa SA_Midassembly/SA_mid_contigstoremove.txt > SA_Midassembly/SA_midfilt2.fa

python filter_fasta_by_list_of_headers.py SR_Midassembly/SR_midfilt.contigs.fa SR_Midassembly/SR_mid_contigstoremove.txt > SR_Midassembly/SR_midfilt2.fa

python filter_fasta_by_list_of_headers.py SS_Midassembly/SS_midfilt.contigs.fa SS_Midassembly/SS_mid_contigstoremove.txt > SS_Midassembly/SS_midfilt2.fa

python filter_fasta_by_list_of_headers.py S3_Fallassembly/S3_fallfilt.contigs.fa S3_Fallassembly/S3_fall_contigstoremove.txt > S3_Fallassembly/S3_fallfilt2.fa

python filter_fasta_by_list_of_headers.py SA_Fallassembly/SA_fallfilt.contigs.fa SA_Fallassembly/SA_fall_contigstoremove.txt > SA_Fallassembly/SA_fallfilt2.fa

python filter_fasta_by_list_of_headers.py SR_Fallassembly/SR_fallfilt.contigs.fa SR_Fallassembly/SR_fall_contigstoremove.txt > SR_Fallassembly/SR_fallfilt2.fa

python filter_fasta_by_list_of_headers.py SS_Fallassembly/SS_fallfilt.contigs.fa SS_Fallassembly/SS_fall_contigstoremove.txt > SS_Fallassembly/SS_fallfilt2.fa

### filter_fasta_by_list_of_headers.py
#!/usr/bin/env python3

from Bio import SeqIO
import sys

ffile = SeqIO.parse(sys.argv[1], "fasta")
header_set = set(line.strip() for line in open(sys.argv[2]))

for seq_record in ffile:
    try:
        header_set.remove(seq_record.name)
    except KeyError:
        print(seq_record.format("fasta"))
        continue
if len(header_set) != 0:
    print(len(header_set),'of the headers from list were not identified in the input fasta file.', file=sys.stderr)


#### Quast on filtered assembly
quast.py S3_Midassembly/S3_midfilt2.fa --threads 32 -o S3_Midassembly/S3_Mid_quastfilt2

quast.py SA_Midassembly/SA_midfilt2.fa --threads 32 -o SA_Midassembly/SA_Mid_quastfilt2

quast.py SR_Midassembly/SR_midfilt2.fa --threads 32 -o SR_Midassembly/SR_Mid_quastfilt2

quast.py SS_Midassembly/SS_midfilt2.fa --threads 32 -o SS_Midassembly/SS_Mid_quastfilt2

quast.py S3_Fallassembly/S3_fallfilt2.fa --threads 32 -o S3_Fallassembly/S3_Fall_quastfilt2

quast.py SA_Fallassembly/SA_fallfilt2.fa --threads 32 -o SA_Fallassembly/SA_Fall_quastfilt2

quast.py SR_Fallassembly/SR_fallfilt2.fa --threads 32 -o SR_Fallassembly/SR_Fall_quastfilt2

quast.py SS_Fallassembly/SS_fallfilt2.fa --threads 32 -o SS_Fallassembly/SS_Fall_quastfilt2

### eukrep to separate eukaryotic and prokaryotic contigs
singularity run --bind $PWD --pwd $PWD /programs/EukRep-0.6.7/eukrep.sif EukRep -i S3_Midassembly/S3_midfilt2.fa -o S3_Midassembly/S3_mideuk.fasta --prokarya S3_Midassembly/S3_midprok.fasta

singularity run --bind $PWD --pwd $PWD /programs/EukRep-0.6.7/eukrep.sif EukRep -i SA_Midassembly/SA_midfilt2.fa -o SA_Midassembly/SA_mideuk.fasta --prokarya SA_Midassembly/SA_midprok.fasta

singularity run --bind $PWD --pwd $PWD /programs/EukRep-0.6.7/eukrep.sif EukRep -i SR_Midassembly/SR_midfilt2.fa -o SR_Midassembly/SR_mideuk.fasta --prokarya SR_Midassembly/SR_midprok.fasta

singularity run --bind $PWD --pwd $PWD /programs/EukRep-0.6.7/eukrep.sif EukRep -i SS_Midassembly/SS_midfilt2.fa -o SS_Midassembly/SS_mideuk.fasta --prokarya SS_Midassembly/SS_midprok.fasta

singularity run --bind $PWD --pwd $PWD /programs/EukRep-0.6.7/eukrep.sif EukRep -i S3_Fallassembly/S3_fallfilt2.fa -o S3_Fallassembly/S3_falleuk.fasta --prokarya S3_Fallassembly/S3_fallprok.fasta

singularity run --bind $PWD --pwd $PWD /programs/EukRep-0.6.7/eukrep.sif EukRep -i SA_Fallassembly/SA_fallfilt2.fa -o SA_Fallassembly/SA_falleuk.fasta --prokarya SA_Fallassembly/SA_fallprok.fasta

singularity run --bind $PWD --pwd $PWD /programs/EukRep-0.6.7/eukrep.sif EukRep -i SR_Fallassembly/SR_fallfilt2.fa -o SR_Fallassembly/SR_falleuk.fasta --prokarya SR_Fallassembly/SR_fallprok.fasta

singularity run --bind $PWD --pwd $PWD /programs/EukRep-0.6.7/eukrep.sif EukRep -i SS_Fallassembly/SS_fallfilt2.fa -o SS_Fallassembly/SS_falleuk.fasta --prokarya SS_Fallassembly/SS_fallprok.fasta

#### Quast on euk and prok fasta's
quast.py S3_Midassembly/S3_mideuk.fasta --threads 32 -o S3_Midassembly/S3_Mid_quasteuk

quast.py SA_Midassembly/SA_mideuk.fasta --threads 32 -o SA_Midassembly/SA_Mid_quasteuk

quast.py SR_Midassembly/SR_mideuk.fasta --threads 32 -o SR_Midassembly/SR_Mid_quasteuk

quast.py SS_Midassembly/SS_mideuk.fasta --threads 32 -o SS_Midassembly/SS_Mid_quasteuk

quast.py S3_Midassembly/S3_midprok.fasta --threads 32 -o S3_Midassembly/S3_Mid_quastprok

quast.py SA_Midassembly/SA_midprok.fasta --threads 32 -o SA_Midassembly/SA_Mid_quastprok

quast.py SR_Midassembly/SR_midprok.fasta --threads 32 -o SR_Midassembly/SR_Mid_quastprok

quast.py SS_Midassembly/SS_midprok.fasta --threads 32 -o SS_Midassembly/SS_Mid_quastprok

quast.py S3_Fallassembly/S3_falleuk.fasta --threads 32 -o S3_Fallassembly/S3_Fall_quasteuk

quast.py SA_Fallassembly/SA_falleuk.fasta --threads 32 -o SA_Fallassembly/SA_Fall_quasteuk

quast.py SR_Fallassembly/SR_falleuk.fasta --threads 32 -o SR_Fallassembly/SR_Fall_quasteuk

quast.py SS_Fallassembly/SS_falleuk.fasta --threads 32 -o SS_Fallassembly/SS_Fall_quasteuk

quast.py S3_Fallassembly/S3_fallprok.fasta --threads 32 -o S3_Fallassembly/S3_Fall_quastprok

quast.py SA_Fallassembly/SA_fallprok.fasta --threads 32 -o SA_Fallassembly/SA_Fall_quastprok

quast.py SR_Fallassembly/SR_fallprok.fasta --threads 32 -o SR_Fallassembly/SR_Fall_quastprok

quast.py SS_Fallassembly/SS_fallprok.fasta --threads 32 -o SS_Fallassembly/SS_Fall_quastprok


### Anvio- use prokaryote file 
anvi-script-reformat-fasta /workdir/cystmetagenome_all/S3_Midassembly/S3_midprok.fasta -o S3midprok-fixed.fa -l 0 --simplify-names

anvi-script-reformat-fasta /workdir/cystmetagenome_all/SA_Midassembly/SA_midprok.fasta -o SAmidprok-fixed.fa -l 0 --simplify-names

anvi-script-reformat-fasta /workdir/cystmetagenome_all/SR_Midassembly/SR_midprok.fasta -o SRmidprok-fixed.fa -l 0 --simplify-names

anvi-script-reformat-fasta /workdir/cystmetagenome_all/SS_Midassembly/SS_midprok.fasta -o SSmidprok-fixed.fa -l 0 --simplify-names

anvi-script-reformat-fasta /workdir/cystmetagenome_all/S3_Fallassembly/S3_fallprok.fasta -o S3fallprok-fixed.fa -l 0 --simplify-names

anvi-script-reformat-fasta /workdir/cystmetagenome_all/SA_Fallassembly/SA_fallprok.fasta -o SAfallprok-fixed.fa -l 0 --simplify-names

anvi-script-reformat-fasta /workdir/cystmetagenome_all/SR_Fallassembly/SR_fallprok.fasta -o SRfallprok-fixed.fa -l 0 --simplify-names

anvi-script-reformat-fasta /workdir/cystmetagenome_all/SS_Fallassembly/SS_fallprok.fasta -o SSfallprok-fixed.fa -l 0 --simplify-names


anvi-gen-contigs-database -f S3midprok-fixed.fa -o S3midprok.db --ignore-internal-stop-codons -n anviS3midprok --num-threads 40

anvi-gen-contigs-database -f SAmidprok-fixed.fa -o SAmidprok.db --ignore-internal-stop-codons -n anviSAmidprok --num-threads 40

anvi-gen-contigs-database -f SRmidprok-fixed.fa -o SRmidprok.db --ignore-internal-stop-codons -n anviSRmidprok --num-threads 40

anvi-gen-contigs-database -f SSmidprok-fixed.fa -o SSmidprok.db --ignore-internal-stop-codons -n anviSSmidprok --num-threads 40

anvi-gen-contigs-database -f S3fallprok-fixed.fa -o S3fallprok.db --ignore-internal-stop-codons -n anviS3fallprok --num-threads 40

anvi-gen-contigs-database -f SAfallprok-fixed.fa -o SAfallprok.db --ignore-internal-stop-codons -n anviSAfallprok --num-threads 40

anvi-gen-contigs-database -f SRfallprok-fixed.fa -o SRfallprok.db --ignore-internal-stop-codons -n anviSRfallprok --num-threads 40

anvi-gen-contigs-database -f SSfallprok-fixed.fa -o SSfallprok.db --ignore-internal-stop-codons -n anviSSfallprok --num-threads 40
 
anvi-run-hmms -c S3midprok.db --num-threads 40
anvi-run-hmms -c SAmidprok.db --num-threads 40
anvi-run-hmms -c SRmidprok.db --num-threads 40
anvi-run-hmms -c SSmidprok.db --num-threads 40
anvi-run-hmms -c S3fallprok.db --num-threads 40
anvi-run-hmms -c SAfallprok.db --num-threads 40
anvi-run-hmms -c SRfallprok.db --num-threads 40
anvi-run-hmms -c SSfallprok.db --num-threads 40

anvi-setup-ncbi-cogs --num-threads 40
anvi-run-ncbi-cogs -c S3midprok.db --num-threads 40
anvi-run-ncbi-cogs -c SAmidprok.db --num-threads 40
anvi-run-ncbi-cogs -c SRmidprok.db --num-threads 40
anvi-run-ncbi-cogs -c SSmidprok.db --num-threads 40
anvi-run-ncbi-cogs -c S3fallprok.db --num-threads 40
anvi-run-ncbi-cogs -c SAfallprok.db --num-threads 40
anvi-run-ncbi-cogs -c SRfallprok.db --num-threads 40
anvi-run-ncbi-cogs -c SSfallprok.db --num-threads 40


### Reciprocal mapping/all vs all mapping. This greatly improves bin completness/contamination

#### Map to S3mid
bwa index S3midprok-fixed.fa 

bwa mem S3midprok-fixed.fa /workdir/cystmetagenome_all/S3cystsmid_R1.fastq.gz /workdir/cystmetagenome_all/S3cystsmid_R2.fastq.gz > S3midprok_aligned.sam -t 40

samtools fixmate -O bam S3midprok_aligned.sam S3midprok_fixmate.bam

samtools sort -o S3midprok_sorted.bam -O bam -T temp S3midprok_fixmate.bam

samtools index S3midprok_sorted.bam

anvi-init-bam S3midprok_sorted.bam -o S3midprok.bam


bwa index SAmidprok-fixed.fa 
bwa mem SAmidprok-fixed.fa /workdir/cystmetagenome_all/S3cystsmid_R1.fastq.gz /workdir/cystmetagenome_all/S3cystsmid_R2.fastq.gz > SAmS3midprok_aligned.sam -t 40

samtools fixmate -O bam SAmS3midprok_aligned.sam SAmS3midprok_fixmate.bam

samtools sort -o SAmS3midprok_sorted.bam -O bam -T temp SAmS3midprok_fixmate.bam

samtools index SAmS3midprok_sorted.bam

anvi-init-bam SAmS3midprok_sorted.bam -o SAmS3midprok.bam


bwa index SRmidprok-fixed.fa 

bwa mem SRmidprok-fixed.fa /workdir/cystmetagenome_all/S3cystsmid_R1.fastq.gz /workdir/cystmetagenome_all/S3cystsmid_R2.fastq.gz > SRmS3midprok_aligned.sam -t 40

samtools fixmate -O bam SRmS3midprok_aligned.sam SRmS3midprok_fixmate.bam

samtools sort -o SRmS3midprok_sorted.bam -O bam -T temp SRmS3midprok_fixmate.bam

samtools index SRmS3midprok_sorted.bam

anvi-init-bam SRmS3midprok_sorted.bam -o SRmS3midprok.bam


bwa index SSmidprok-fixed.fa 

bwa mem SSmidprok-fixed.fa /workdir/cystmetagenome_all/S3cystsmid_R1.fastq.gz /workdir/cystmetagenome_all/S3cystsmid_R2.fastq.gz > SSmS3midprok_aligned.sam -t 40

samtools fixmate -O bam SSmS3midprok_aligned.sam SSmS3midprok_fixmate.bam

samtools sort -o SSmS3midprok_sorted.bam -O bam -T temp SSmS3midprok_fixmate.bam

samtools index SSmS3midprok_sorted.bam

anvi-init-bam SSmS3midprok_sorted.bam -o SSmS3midprok.bam


bwa index S3fallprok-fixed.fa 

bwa mem S3fallprok-fixed.fa /workdir/cystmetagenome_all/S3cystsmid_R1.fastq.gz /workdir/cystmetagenome_all/S3cystsmid_R2.fastq.gz > S3fS3midprok_aligned.sam -t 40

samtools fixmate -O bam S3fS3midprok_aligned.sam S3fS3midprok_fixmate.bam

samtools sort -o S3fS3midprok_sorted.bam -O bam -T temp S3fS3midprok_fixmate.bam

samtools index S3fS3midprok_sorted.bam

anvi-init-bam S3fS3midprok_sorted.bam -o S3fS3midprok.bam


bwa index SAfallprok-fixed.fa 
bwa mem SAfallprok-fixed.fa /workdir/cystmetagenome_all/S3cystsmid_R1.fastq.gz /workdir/cystmetagenome_all/S3cystsmid_R2.fastq.gz > SAfS3midprok_aligned.sam -t 40

samtools fixmate -O bam SAfS3midprok_aligned.sam SAfS3midprok_fixmate.bam
samtools sort -o SAfS3midprok_sorted.bam -O bam -T temp SAfS3midprok_fixmate.bam
samtools index SAfS3midprok_sorted.bam
anvi-init-bam SAfS3midprok_sorted.bam -o SAfS3midprok.bam

bwa index SRfallprok-fixed.fa 
bwa mem SRfallprok-fixed.fa /workdir/cystmetagenome_all/S3cystsmid_R1.fastq.gz /workdir/cystmetagenome_all/S3cystsmid_R2.fastq.gz > SRfS3midprok_aligned.sam -t 40

samtools fixmate -O bam SRfS3midprok_aligned.sam SRfS3midprok_fixmate.bam
samtools sort -o SRfS3midprok_sorted.bam -O bam -T temp SRfS3midprok_fixmate.bam
samtools index SRfS3midprok_sorted.bam
anvi-init-bam SRfS3midprok_sorted.bam -o SRfS3midprok.bam

bwa index SSfallprok-fixed.fa 
bwa mem SSfallprok-fixed.fa /workdir/cystmetagenome_all/S3cystsmid_R1.fastq.gz /workdir/cystmetagenome_all/S3cystsmid_R2.fastq.gz > SSfS3midprok_aligned.sam -t 40
samtools fixmate -O bam SSfS3midprok_aligned.sam SSfS3midprok_fixmate.bam
samtools sort -o SSfS3midprok_sorted.bam -O bam -T temp SSfS3midprok_fixmate.bam
samtools index SSfS3midprok_sorted.bam
anvi-init-bam SSfS3midprok_sorted.bam -o SSfS3midprok.bam



#skipping the bwa index step now that all files have been indexed
#### Map to SAmid
bwa mem SAmidprok-fixed.fa /workdir/cystmetagenome_all/SAcystsmid_R1.fastq.gz /workdir/cystmetagenome_all/SAcystsmid_R2.fastq.gz > SAmidprok_aligned.sam -t 40
samtools fixmate -O bam SAmidprok_aligned.sam SAmidprok_fixmate.bam
samtools sort -o SAmidprok_sorted.bam -O bam -T temp SAmidprok_fixmate.bam
samtools index SAmidprok_sorted.bam
anvi-init-bam SAmidprok_sorted.bam -o SAmidprok.bam

bwa mem S3midprok-fixed.fa /workdir/cystmetagenome_all/SAcystsmid_R1.fastq.gz /workdir/cystmetagenome_all/SAcystsmid_R2.fastq.gz > S3mSAmidprok_aligned.sam -t 40
samtools fixmate -O bam S3mSAmidprok_aligned.sam S3mSAmidprok_fixmate.bam
samtools sort -o S3mSAmidprok_sorted.bam -O bam -T temp S3mSAmidprok_fixmate.bam
samtools index S3mSAmidprok_sorted.bam
anvi-init-bam S3mSAmidprok_sorted.bam -o S3mSAmidprok.bam

bwa mem SRmidprok-fixed.fa /workdir/cystmetagenome_all/SAcystsmid_R1.fastq.gz /workdir/cystmetagenome_all/SAcystsmid_R2.fastq.gz > SRmSAmidprok_aligned.sam -t 40
samtools fixmate -O bam SRmSAmidprok_aligned.sam SRmSAmidprok_fixmate.bam
samtools sort -o SRmSAmidprok_sorted.bam -O bam -T temp SRmSAmidprok_fixmate.bam
samtools index SRmSAmidprok_sorted.bam
anvi-init-bam SRmSAmidprok_sorted.bam -o SRmSAmidprok.bam

bwa mem SSmidprok-fixed.fa /workdir/cystmetagenome_all/SAcystsmid_R1.fastq.gz /workdir/cystmetagenome_all/SAcystsmid_R2.fastq.gz > SSmSAmidprok_aligned.sam -t 40
samtools fixmate -O bam SSmSAmidprok_aligned.sam SSmSAmidprok_fixmate.bam
samtools sort -o SSmSAmidprok_sorted.bam -O bam -T temp SSmSAmidprok_fixmate.bam
samtools index SSmSAmidprok_sorted.bam
anvi-init-bam SSmSAmidprok_sorted.bam -o SSmSAmidprok.bam

bwa mem S3fallprok-fixed.fa /workdir/cystmetagenome_all/SAcystsmid_R1.fastq.gz /workdir/cystmetagenome_all/SAcystsmid_R2.fastq.gz > S3fSAmidprok_aligned.sam -t 40
samtools fixmate -O bam S3fSAmidprok_aligned.sam S3fSAmidprok_fixmate.bam
samtools sort -o S3fSAmidprok_sorted.bam -O bam -T temp S3fSAmidprok_fixmate.bam
samtools index S3fSAmidprok_sorted.bam
anvi-init-bam S3fSAmidprok_sorted.bam -o S3fSAmidprok.bam

bwa mem SAfallprok-fixed.fa /workdir/cystmetagenome_all/SAcystsmid_R1.fastq.gz /workdir/cystmetagenome_all/SAcystsmid_R2.fastq.gz > SAfSAmidprok_aligned.sam -t 40
samtools fixmate -O bam SAfSAmidprok_aligned.sam SAfSAmidprok_fixmate.bam
samtools sort -o SAfSAmidprok_sorted.bam -O bam -T temp SAfSAmidprok_fixmate.bam
samtools index SAfSAmidprok_sorted.bam
anvi-init-bam SAfSAmidprok_sorted.bam -o SAfSAmidprok.bam

bwa mem SRfallprok-fixed.fa /workdir/cystmetagenome_all/SAcystsmid_R1.fastq.gz /workdir/cystmetagenome_all/SAcystsmid_R2.fastq.gz > SRfSAmidprok_aligned.sam -t 40
samtools fixmate -O bam SRfSAmidprok_aligned.sam SRfSAmidprok_fixmate.bam
samtools sort -o SRfSAmidprok_sorted.bam -O bam -T temp SRfSAmidprok_fixmate.bam
samtools index SRfSAmidprok_sorted.bam
anvi-init-bam SRfSAmidprok_sorted.bam -o SRfSAmidprok.bam

bwa mem SSfallprok-fixed.fa /workdir/cystmetagenome_all/SAcystsmid_R1.fastq.gz /workdir/cystmetagenome_all/SAcystsmid_R2.fastq.gz > SSfSAmidprok_aligned.sam -t 40
samtools fixmate -O bam SSfSAmidprok_aligned.sam SSfSAmidprok_fixmate.bam
samtools sort -o SSfSAmidprok_sorted.bam -O bam -T temp SSfSAmidprok_fixmate.bam
samtools index SSfSAmidprok_sorted.bam
anvi-init-bam SSfSAmidprok_sorted.bam -o SSfSAmidprok.bam


#### Map to SRmid
bwa mem SRmidprok-fixed.fa /workdir/cystmetagenome_all/SRcystsmid_R1.fastq.gz /workdir/cystmetagenome_all/SRcystsmid_R2.fastq.gz > SRmidprok_aligned.sam -t 40
samtools fixmate -O bam SRmidprok_aligned.sam SRmidprok_fixmate.bam
samtools sort -o SRmidprok_sorted.bam -O bam -T temp SRmidprok_fixmate.bam
samtools index SRmidprok_sorted.bam
anvi-init-bam SRmidprok_sorted.bam -o SRmidprok.bam

bwa mem S3midprok-fixed.fa /workdir/cystmetagenome_all/SRcystsmid_R1.fastq.gz /workdir/cystmetagenome_all/SRcystsmid_R2.fastq.gz > S3mSRmidprok_aligned.sam -t 40
samtools fixmate -O bam S3mSRmidprok_aligned.sam S3mSRmidprok_fixmate.bam
samtools sort -o S3mSRmidprok_sorted.bam -O bam -T temp S3mSRmidprok_fixmate.bam
samtools index S3mSRmidprok_sorted.bam
anvi-init-bam S3mSRmidprok_sorted.bam -o S3mSRmidprok.bam

bwa mem SAmidprok-fixed.fa /workdir/cystmetagenome_all/SRcystsmid_R1.fastq.gz /workdir/cystmetagenome_all/SRcystsmid_R2.fastq.gz > SAmSRmidprok_aligned.sam -t 40
samtools fixmate -O bam SAmSRmidprok_aligned.sam SAmSRmidprok_fixmate.bam
samtools sort -o SAmSRmidprok_sorted.bam -O bam -T temp SAmSRmidprok_fixmate.bam
samtools index SAmSRmidprok_sorted.bam
anvi-init-bam SAmSRmidprok_sorted.bam -o SAmSRmidprok.bam

bwa mem SSmidprok-fixed.fa /workdir/cystmetagenome_all/SRcystsmid_R1.fastq.gz /workdir/cystmetagenome_all/SRcystsmid_R2.fastq.gz > SSmSRmidprok_aligned.sam -t 40
samtools fixmate -O bam SSmSRmidprok_aligned.sam SSmSRmidprok_fixmate.bam
samtools sort -o SSmSRmidprok_sorted.bam -O bam -T temp SSmSRmidprok_fixmate.bam
samtools index SSmSRmidprok_sorted.bam
anvi-init-bam SSmSRmidprok_sorted.bam -o SSmSRmidprok.bam

bwa mem S3fallprok-fixed.fa /workdir/cystmetagenome_all/SRcystsmid_R1.fastq.gz /workdir/cystmetagenome_all/SRcystsmid_R2.fastq.gz > S3fSRmidprok_aligned.sam -t 40
samtools fixmate -O bam S3fSRmidprok_aligned.sam S3fSRmidprok_fixmate.bam
samtools sort -o S3fSRmidprok_sorted.bam -O bam -T temp S3fSRmidprok_fixmate.bam
samtools index S3fSRmidprok_sorted.bam
anvi-init-bam S3fSRmidprok_sorted.bam -o S3fSRmidprok.bam

bwa mem SAfallprok-fixed.fa /workdir/cystmetagenome_all/SRcystsmid_R1.fastq.gz /workdir/cystmetagenome_all/SRcystsmid_R2.fastq.gz > SAfSRmidprok_aligned.sam -t 40
samtools fixmate -O bam SAfSRmidprok_aligned.sam SAfSRmidprok_fixmate.bam
samtools sort -o SAfSRmidprok_sorted.bam -O bam -T temp SAfSRmidprok_fixmate.bam
samtools index SAfSRmidprok_sorted.bam
anvi-init-bam SAfSRmidprok_sorted.bam -o SAfSRmidprok.bam

bwa mem SRfallprok-fixed.fa /workdir/cystmetagenome_all/SRcystsmid_R1.fastq.gz /workdir/cystmetagenome_all/SRcystsmid_R2.fastq.gz > SRfSRmidprok_aligned.sam -t 40
samtools fixmate -O bam SRfSRmidprok_aligned.sam SRfSRmidprok_fixmate.bam
samtools sort -o SRfSRmidprok_sorted.bam -O bam -T temp SRfSRmidprok_fixmate.bam
samtools index SRfSRmidprok_sorted.bam
anvi-init-bam SRfSRmidprok_sorted.bam -o SRfSRmidprok.bam

bwa mem SSfallprok-fixed.fa /workdir/cystmetagenome_all/SRcystsmid_R1.fastq.gz /workdir/cystmetagenome_all/SRcystsmid_R2.fastq.gz > SSfSRmidprok_aligned.sam -t 40
samtools fixmate -O bam SSfSRmidprok_aligned.sam SSfSRmidprok_fixmate.bam
samtools sort -o SSfSRmidprok_sorted.bam -O bam -T temp SSfSRmidprok_fixmate.bam
samtools index SSfSRmidprok_sorted.bam
anvi-init-bam SSfSRmidprok_sorted.bam -o SSfSRmidprok.bam


#### Map to SSmid
bwa mem SSmidprok-fixed.fa /workdir/cystmetagenome_all/SScystsmid_R1.fastq.gz /workdir/cystmetagenome_all/SScystsmid_R2.fastq.gz > SSmidprok_aligned.sam -t 40
samtools fixmate -O bam SSmidprok_aligned.sam SSmidprok_fixmate.bam
samtools sort -o SSmidprok_sorted.bam -O bam -T temp SSmidprok_fixmate.bam
samtools index SSmidprok_sorted.bam
anvi-init-bam SSmidprok_sorted.bam -o SSmidprok.bam

bwa mem S3midprok-fixed.fa /workdir/cystmetagenome_all/SScystsmid_R1.fastq.gz /workdir/cystmetagenome_all/SScystsmid_R2.fastq.gz > S3mSSmidprok_aligned.sam -t 40
samtools fixmate -O bam S3mSSmidprok_aligned.sam S3mSSmidprok_fixmate.bam
samtools sort -o S3mSSmidprok_sorted.bam -O bam -T temp S3mSSmidprok_fixmate.bam
samtools index S3mSSmidprok_sorted.bam
anvi-init-bam S3mSSmidprok_sorted.bam -o S3mSSmidprok.bam

bwa mem SAmidprok-fixed.fa /workdir/cystmetagenome_all/SScystsmid_R1.fastq.gz /workdir/cystmetagenome_all/SScystsmid_R2.fastq.gz > SAmSSmidprok_aligned.sam -t 40
samtools fixmate -O bam SAmSSmidprok_aligned.sam SAmSSmidprok_fixmate.bam
samtools sort -o SAmSSmidprok_sorted.bam -O bam -T temp SAmSSmidprok_fixmate.bam
samtools index SAmSSmidprok_sorted.bam
anvi-init-bam SAmSSmidprok_sorted.bam -o SAmSSmidprok.bam

bwa mem SRmidprok-fixed.fa /workdir/cystmetagenome_all/SScystsmid_R1.fastq.gz /workdir/cystmetagenome_all/SScystsmid_R2.fastq.gz > SRmSSmidprok_aligned.sam -t 40
samtools fixmate -O bam SRmSSmidprok_aligned.sam SRmSSmidprok_fixmate.bam
samtools sort -o SRmSSmidprok_sorted.bam -O bam -T temp SRmSSmidprok_fixmate.bam
samtools index SRmSSmidprok_sorted.bam
anvi-init-bam SRmSSmidprok_sorted.bam -o SRmSSmidprok.bam

bwa mem S3fallprok-fixed.fa /workdir/cystmetagenome_all/SScystsmid_R1.fastq.gz /workdir/cystmetagenome_all/SScystsmid_R2.fastq.gz > S3fSSmidprok_aligned.sam -t 40
samtools fixmate -O bam S3fSSmidprok_aligned.sam S3fSSmidprok_fixmate.bam
samtools sort -o S3fSSmidprok_sorted.bam -O bam -T temp S3fSSmidprok_fixmate.bam
samtools index S3fSSmidprok_sorted.bam
anvi-init-bam S3fSSmidprok_sorted.bam -o S3fSSmidprok.bam

bwa mem SAfallprok-fixed.fa /workdir/cystmetagenome_all/SScystsmid_R1.fastq.gz /workdir/cystmetagenome_all/SScystsmid_R2.fastq.gz > SAfSSmidprok_aligned.sam -t 40
samtools fixmate -O bam SAfSSmidprok_aligned.sam SAfSSmidprok_fixmate.bam
samtools sort -o SAfSSmidprok_sorted.bam -O bam -T temp SAfSSmidprok_fixmate.bam
samtools index SAfSSmidprok_sorted.bam
anvi-init-bam SAfSSmidprok_sorted.bam -o SAfSSmidprok.bam

bwa mem SRfallprok-fixed.fa /workdir/cystmetagenome_all/SScystsmid_R1.fastq.gz /workdir/cystmetagenome_all/SScystsmid_R2.fastq.gz > SRfSSmidprok_aligned.sam -t 40
samtools fixmate -O bam SRfSSmidprok_aligned.sam SRfSSmidprok_fixmate.bam
samtools sort -o SRfSSmidprok_sorted.bam -O bam -T temp SRfSSmidprok_fixmate.bam
samtools index SRfSSmidprok_sorted.bam
anvi-init-bam SRfSSmidprok_sorted.bam -o SRfSSmidprok.bam

bwa mem SSfallprok-fixed.fa /workdir/cystmetagenome_all/SScystsmid_R1.fastq.gz /workdir/cystmetagenome_all/SScystsmid_R2.fastq.gz > SSfSSmidprok_aligned.sam -t 40
samtools fixmate -O bam SSfSSmidprok_aligned.sam SSfSSmidprok_fixmate.bam
samtools sort -o SSfSSmidprok_sorted.bam -O bam -T temp SSfSSmidprok_fixmate.bam
samtools index SSfSSmidprok_sorted.bam
anvi-init-bam SSfSSmidprok_sorted.bam -o SSfSSmidprok.bam


#### Map to S3fall
bwa mem S3fallprok-fixed.fa /workdir/cystmetagenome_all/S3cystsfall_R1.fastq.gz /workdir/cystmetagenome_all/S3cystsfall_R2.fastq.gz > S3fallprok_aligned.sam -t 40
samtools fixmate -O bam S3fallprok_aligned.sam S3fallprok_fixmate.bam
samtools sort -o S3fallprok_sorted.bam -O bam -T temp S3fallprok_fixmate.bam
samtools index S3fallprok_sorted.bam
anvi-init-bam S3fallprok_sorted.bam -o S3fallprok.bam

bwa mem S3midprok-fixed.fa /workdir/cystmetagenome_all/S3cystsfall_R1.fastq.gz /workdir/cystmetagenome_all/S3cystsfall_R2.fastq.gz > S3mS3fallprok_aligned.sam -t 40
samtools fixmate -O bam S3mS3fallprok_aligned.sam S3mS3fallprok_fixmate.bam
samtools sort -o S3mS3fallprok_sorted.bam -O bam -T temp S3mS3fallprok_fixmate.bam
samtools index S3mS3fallprok_sorted.bam
anvi-init-bam S3mS3fallprok_sorted.bam -o S3mS3fallprok.bam

bwa mem SAmidprok-fixed.fa /workdir/cystmetagenome_all/S3cystsfall_R1.fastq.gz /workdir/cystmetagenome_all/S3cystsfall_R2.fastq.gz > SAmS3fallprok_aligned.sam -t 40
samtools fixmate -O bam SAmS3fallprok_aligned.sam SAmS3fallprok_fixmate.bam
samtools sort -o SAmS3fallprok_sorted.bam -O bam -T temp SAmS3fallprok_fixmate.bam
samtools index SAmS3fallprok_sorted.bam
anvi-init-bam SAmS3fallprok_sorted.bam -o SAmS3fallprok.bam

bwa mem SRmidprok-fixed.fa /workdir/cystmetagenome_all/S3cystsfall_R1.fastq.gz /workdir/cystmetagenome_all/S3cystsfall_R2.fastq.gz > SRmS3fallprok_aligned.sam -t 40
samtools fixmate -O bam SRmS3fallprok_aligned.sam SRmS3fallprok_fixmate.bam
samtools sort -o SRmS3fallprok_sorted.bam -O bam -T temp SRmS3fallprok_fixmate.bam
samtools index SRmS3fallprok_sorted.bam
anvi-init-bam SRmS3fallprok_sorted.bam -o SRmS3fallprok.bam

bwa mem SSmidprok-fixed.fa /workdir/cystmetagenome_all/S3cystsfall_R1.fastq.gz /workdir/cystmetagenome_all/S3cystsfall_R2.fastq.gz > SSmS3fallprok_aligned.sam -t 40
samtools fixmate -O bam SSmS3fallprok_aligned.sam SSmS3fallprok_fixmate.bam
samtools sort -o SSmS3fallprok_sorted.bam -O bam -T temp SSmS3fallprok_fixmate.bam
samtools index SSmS3fallprok_sorted.bam
anvi-init-bam SSmS3fallprok_sorted.bam -o SSmS3fallprok.bam

bwa mem SAfallprok-fixed.fa /workdir/cystmetagenome_all/S3cystsfall_R1.fastq.gz /workdir/cystmetagenome_all/S3cystsfall_R2.fastq.gz > SAfS3fallprok_aligned.sam -t 40
samtools fixmate -O bam SAfS3fallprok_aligned.sam SAfS3fallprok_fixmate.bam
samtools sort -o SAfS3fallprok_sorted.bam -O bam -T temp SAfS3fallprok_fixmate.bam
samtools index SAfS3fallprok_sorted.bam
anvi-init-bam SAfS3fallprok_sorted.bam -o SAfS3fallprok.bam

bwa mem SRfallprok-fixed.fa /workdir/cystmetagenome_all/S3cystsfall_R1.fastq.gz /workdir/cystmetagenome_all/S3cystsfall_R2.fastq.gz > SRfS3fallprok_aligned.sam -t 40
samtools fixmate -O bam SRfS3fallprok_aligned.sam SRfS3fallprok_fixmate.bam
samtools sort -o SRfS3fallprok_sorted.bam -O bam -T temp SRfS3fallprok_fixmate.bam
samtools index SRfS3fallprok_sorted.bam
anvi-init-bam SRfS3fallprok_sorted.bam -o SRfS3fallprok.bam

bwa mem SSfallprok-fixed.fa /workdir/cystmetagenome_all/S3cystsfall_R1.fastq.gz /workdir/cystmetagenome_all/S3cystsfall_R2.fastq.gz > SSfS3fallprok_aligned.sam -t 40
samtools fixmate -O bam SSfS3fallprok_aligned.sam SSfS3fallprok_fixmate.bam
samtools sort -o SSfS3fallprok_sorted.bam -O bam -T temp SSfS3fallprok_fixmate.bam
samtools index SSfS3fallprok_sorted.bam
anvi-init-bam SSfS3fallprok_sorted.bam -o SSfS3fallprok.bam


#### Map to SAfall
bwa mem SAfallprok-fixed.fa /workdir/cystmetagenome_all/SAcystsfall_R1.fastq.gz /workdir/cystmetagenome_all/SAcystsfall_R2.fastq.gz > SAfallprok_aligned.sam -t 40
samtools fixmate -O bam SAfallprok_aligned.sam SAfallprok_fixmate.bam
samtools sort -o SAfallprok_sorted.bam -O bam -T temp SAfallprok_fixmate.bam
samtools index SAfallprok_sorted.bam
anvi-init-bam SAfallprok_sorted.bam -o SAfallprok.bam

bwa mem S3midprok-fixed.fa /workdir/cystmetagenome_all/SAcystsfall_R1.fastq.gz /workdir/cystmetagenome_all/SAcystsfall_R2.fastq.gz > S3mSAfallprok_aligned.sam -t 40
samtools fixmate -O bam S3mSAfallprok_aligned.sam S3mSAfallprok_fixmate.bam
samtools sort -o S3mSAfallprok_sorted.bam -O bam -T temp S3mSAfallprok_fixmate.bam
samtools index S3mSAfallprok_sorted.bam
anvi-init-bam S3mSAfallprok_sorted.bam -o S3mSAfallprok.bam

bwa mem SAmidprok-fixed.fa /workdir/cystmetagenome_all/SAcystsfall_R1.fastq.gz /workdir/cystmetagenome_all/SAcystsfall_R2.fastq.gz > SAmSAfallprok_aligned.sam -t 40
samtools fixmate -O bam SAmSAfallprok_aligned.sam SAmSAfallprok_fixmate.bam
samtools sort -o SAmSAfallprok_sorted.bam -O bam -T temp SAmSAfallprok_fixmate.bam
samtools index SAmSAfallprok_sorted.bam
anvi-init-bam SAmSAfallprok_sorted.bam -o SAmSAfallprok.bam

bwa mem SRmidprok-fixed.fa /workdir/cystmetagenome_all/SAcystsfall_R1.fastq.gz /workdir/cystmetagenome_all/SAcystsfall_R2.fastq.gz > SRmSAfallprok_aligned.sam -t 40
samtools fixmate -O bam SRmSAfallprok_aligned.sam SRmSAfallprok_fixmate.bam
samtools sort -o SRmSAfallprok_sorted.bam -O bam -T temp SRmSAfallprok_fixmate.bam
samtools index SRmSAfallprok_sorted.bam
anvi-init-bam SRmSAfallprok_sorted.bam -o SRmSAfallprok.bam

bwa mem SSmidprok-fixed.fa /workdir/cystmetagenome_all/SAcystsfall_R1.fastq.gz /workdir/cystmetagenome_all/SAcystsfall_R2.fastq.gz > SSmSAfallprok_aligned.sam -t 40
samtools fixmate -O bam SSmSAfallprok_aligned.sam SSmSAfallprok_fixmate.bam
samtools sort -o SSmSAfallprok_sorted.bam -O bam -T temp SSmSAfallprok_fixmate.bam
samtools index SSmSAfallprok_sorted.bam
anvi-init-bam SSmSAfallprok_sorted.bam -o SSmSAfallprok.bam

bwa mem S3fallprok-fixed.fa /workdir/cystmetagenome_all/SAcystsfall_R1.fastq.gz /workdir/cystmetagenome_all/SAcystsfall_R2.fastq.gz > S3fSAfallprok_aligned.sam -t 40
samtools fixmate -O bam S3fSAfallprok_aligned.sam S3fSAfallprok_fixmate.bam
samtools sort -o S3fSAfallprok_sorted.bam -O bam -T temp S3fSAfallprok_fixmate.bam
samtools index S3fSAfallprok_sorted.bam
anvi-init-bam S3fSAfallprok_sorted.bam -o S3fSAfallprok.bam

bwa mem SRfallprok-fixed.fa /workdir/cystmetagenome_all/SAcystsfall_R1.fastq.gz /workdir/cystmetagenome_all/SAcystsfall_R2.fastq.gz > SRfSAfallprok_aligned.sam -t 40
samtools fixmate -O bam SRfSAfallprok_aligned.sam SRfSAfallprok_fixmate.bam
samtools sort -o SRfSAfallprok_sorted.bam -O bam -T temp SRfSAfallprok_fixmate.bam
samtools index SRfSAfallprok_sorted.bam
anvi-init-bam SRfSAfallprok_sorted.bam -o SRfSAfallprok.bam

bwa mem SSfallprok-fixed.fa /workdir/cystmetagenome_all/SAcystsfall_R1.fastq.gz /workdir/cystmetagenome_all/SAcystsfall_R2.fastq.gz > SSfSAfallprok_aligned.sam -t 40
samtools fixmate -O bam SSfSAfallprok_aligned.sam SSfSAfallprok_fixmate.bam
samtools sort -o SSfSAfallprok_sorted.bam -O bam -T temp SSfSAfallprok_fixmate.bam
samtools index SSfSAfallprok_sorted.bam
anvi-init-bam SSfSAfallprok_sorted.bam -o SSfSAfallprok.bam


#### Map to SRfall
bwa mem SRfallprok-fixed.fa /workdir/cystmetagenome_all/SRcystsfall_R1.fastq.gz /workdir/cystmetagenome_all/SRcystsfall_R2.fastq.gz > SRfallprok_aligned.sam -t 40
samtools fixmate -O bam SRfallprok_aligned.sam SRfallprok_fixmate.bam
samtools sort -o SRfallprok_sorted.bam -O bam -T temp SRfallprok_fixmate.bam
samtools index SRfallprok_sorted.bam
anvi-init-bam SRfallprok_sorted.bam -o SRfallprok.bam

bwa mem S3midprok-fixed.fa /workdir/cystmetagenome_all/SRcystsfall_R1.fastq.gz /workdir/cystmetagenome_all/SRcystsfall_R2.fastq.gz > S3mSRfallprok_aligned.sam -t 40
samtools fixmate -O bam S3mSRfallprok_aligned.sam S3mSRfallprok_fixmate.bam
samtools sort -o S3mSRfallprok_sorted.bam -O bam -T temp S3mSRfallprok_fixmate.bam
samtools index S3mSRfallprok_sorted.bam
anvi-init-bam S3mSRfallprok_sorted.bam -o S3mSRfallprok.bam

bwa mem SAmidprok-fixed.fa /workdir/cystmetagenome_all/SRcystsfall_R1.fastq.gz /workdir/cystmetagenome_all/SRcystsfall_R2.fastq.gz > SAmSRfallprok_aligned.sam -t 40
samtools fixmate -O bam SAmSRfallprok_aligned.sam SAmSRfallprok_fixmate.bam
samtools sort -o SAmSRfallprok_sorted.bam -O bam -T temp SAmSRfallprok_fixmate.bam
samtools index SAmSRfallprok_sorted.bam
anvi-init-bam SAmSRfallprok_sorted.bam -o SAmSRfallprok.bam

bwa mem SRmidprok-fixed.fa /workdir/cystmetagenome_all/SRcystsfall_R1.fastq.gz /workdir/cystmetagenome_all/SRcystsfall_R2.fastq.gz > SRmSRfallprok_aligned.sam -t 40
samtools fixmate -O bam SRmSRfallprok_aligned.sam SRmSRfallprok_fixmate.bam
samtools sort -o SRmSRfallprok_sorted.bam -O bam -T temp SRmSRfallprok_fixmate.bam
samtools index SRmSRfallprok_sorted.bam
anvi-init-bam SRmSRfallprok_sorted.bam -o SRmSRfallprok.bam

bwa mem SSmidprok-fixed.fa /workdir/cystmetagenome_all/SRcystsfall_R1.fastq.gz /workdir/cystmetagenome_all/SRcystsfall_R2.fastq.gz > SSmSRfallprok_aligned.sam -t 40
samtools fixmate -O bam SSmSRfallprok_aligned.sam SSmSRfallprok_fixmate.bam
samtools sort -o SSmSRfallprok_sorted.bam -O bam -T temp SSmSRfallprok_fixmate.bam
samtools index SSmSRfallprok_sorted.bam
anvi-init-bam SSmSRfallprok_sorted.bam -o SSmSRfallprok.bam

bwa mem S3fallprok-fixed.fa /workdir/cystmetagenome_all/SRcystsfall_R1.fastq.gz /workdir/cystmetagenome_all/SRcystsfall_R2.fastq.gz > S3fSRfallprok_aligned.sam -t 40
samtools fixmate -O bam S3fSRfallprok_aligned.sam S3fSRfallprok_fixmate.bam
samtools sort -o S3fSRfallprok_sorted.bam -O bam -T temp S3fSRfallprok_fixmate.bam
samtools index S3fSRfallprok_sorted.bam
anvi-init-bam S3fSRfallprok_sorted.bam -o S3fSRfallprok.bam

bwa mem SAfallprok-fixed.fa /workdir/cystmetagenome_all/SRcystsfall_R1.fastq.gz /workdir/cystmetagenome_all/SRcystsfall_R2.fastq.gz > SAfSRfallprok_aligned.sam -t 40
samtools fixmate -O bam SAfSRfallprok_aligned.sam SAfSRfallprok_fixmate.bam
samtools sort -o SAfSRfallprok_sorted.bam -O bam -T temp SAfSRfallprok_fixmate.bam
samtools index SAfSRfallprok_sorted.bam
anvi-init-bam SAfSRfallprok_sorted.bam -o SAfSRfallprok.bam

bwa mem SSfallprok-fixed.fa /workdir/cystmetagenome_all/SRcystsfall_R1.fastq.gz /workdir/cystmetagenome_all/SRcystsfall_R2.fastq.gz > SSfSRfallprok_aligned.sam -t 40
samtools fixmate -O bam SSfSRfallprok_aligned.sam SSfSRfallprok_fixmate.bam
samtools sort -o SSfSRfallprok_sorted.bam -O bam -T temp SSfSRfallprok_fixmate.bam
samtools index SSfSRfallprok_sorted.bam
anvi-init-bam SSfSRfallprok_sorted.bam -o SSfSRfallprok.bam


#### Map to SSfall
bwa mem SSfallprok-fixed.fa /workdir/cystmetagenome_all/SScystsfall_R1.fastq.gz /workdir/cystmetagenome_all/SScystsfall_R2.fastq.gz > SSfallprok_aligned.sam -t 40
samtools fixmate -O bam SSfallprok_aligned.sam SSfallprok_fixmate.bam
samtools sort -o SSfallprok_sorted.bam -O bam -T temp SSfallprok_fixmate.bam
samtools index SSfallprok_sorted.bam
anvi-init-bam SSfallprok_sorted.bam -o SSfallprok.bam

bwa mem S3midprok-fixed.fa /workdir/cystmetagenome_all/SScystsfall_R1.fastq.gz /workdir/cystmetagenome_all/SScystsfall_R2.fastq.gz > S3mSSfallprok_aligned.sam -t 40
samtools fixmate -O bam S3mSSfallprok_aligned.sam S3mSSfallprok_fixmate.bam
samtools sort -o S3mSSfallprok_sorted.bam -O bam -T temp S3mSSfallprok_fixmate.bam
samtools index S3mSSfallprok_sorted.bam
anvi-init-bam S3mSSfallprok_sorted.bam -o S3mSSfallprok.bam

bwa mem SAmidprok-fixed.fa /workdir/cystmetagenome_all/SScystsfall_R1.fastq.gz /workdir/cystmetagenome_all/SScystsfall_R2.fastq.gz > SAmSSfallprok_aligned.sam -t 40
samtools fixmate -O bam SAmSSfallprok_aligned.sam SAmSSfallprok_fixmate.bam
samtools sort -o SAmSSfallprok_sorted.bam -O bam -T temp SAmSSfallprok_fixmate.bam
samtools index SAmSSfallprok_sorted.bam
anvi-init-bam SAmSSfallprok_sorted.bam -o SAmSSfallprok.bam

bwa mem SRmidprok-fixed.fa /workdir/cystmetagenome_all/SScystsfall_R1.fastq.gz /workdir/cystmetagenome_all/SScystsfall_R2.fastq.gz > SRmSSfallprok_aligned.sam -t 40
samtools fixmate -O bam SRmSSfallprok_aligned.sam SRmSSfallprok_fixmate.bam
samtools sort -o SRmSSfallprok_sorted.bam -O bam -T temp SRmSSfallprok_fixmate.bam
samtools index SRmSSfallprok_sorted.bam
anvi-init-bam SRmSSfallprok_sorted.bam -o SRmSSfallprok.bam

bwa mem SSmidprok-fixed.fa /workdir/cystmetagenome_all/SScystsfall_R1.fastq.gz /workdir/cystmetagenome_all/SScystsfall_R2.fastq.gz > SSmSSfallprok_aligned.sam -t 40
samtools fixmate -O bam SSmSSfallprok_aligned.sam SSmSSfallprok_fixmate.bam
samtools sort -o SSmSSfallprok_sorted.bam -O bam -T temp SSmSSfallprok_fixmate.bam
samtools index SSmSSfallprok_sorted.bam
anvi-init-bam SSmSSfallprok_sorted.bam -o SSmSSfallprok.bam

bwa mem S3fallprok-fixed.fa /workdir/cystmetagenome_all/SScystsfall_R1.fastq.gz /workdir/cystmetagenome_all/SScystsfall_R2.fastq.gz > S3fSSfallprok_aligned.sam -t 40
samtools fixmate -O bam S3fSSfallprok_aligned.sam S3fSSfallprok_fixmate.bam
samtools sort -o S3fSSfallprok_sorted.bam -O bam -T temp S3fSSfallprok_fixmate.bam
samtools index S3fSSfallprok_sorted.bam
anvi-init-bam S3fSSfallprok_sorted.bam -o S3fSSfallprok.bam

bwa mem SAfallprok-fixed.fa /workdir/cystmetagenome_all/SScystsfall_R1.fastq.gz /workdir/cystmetagenome_all/SScystsfall_R2.fastq.gz > SAfSSfallprok_aligned.sam -t 40
samtools fixmate -O bam SAfSSfallprok_aligned.sam SAfSSfallprok_fixmate.bam
samtools sort -o SAfSSfallprok_sorted.bam -O bam -T temp SAfSSfallprok_fixmate.bam
samtools index SAfSSfallprok_sorted.bam
anvi-init-bam SAfSSfallprok_sorted.bam -o SAfSSfallprok.bam

bwa mem SRfallprok-fixed.fa /workdir/cystmetagenome_all/SScystsfall_R1.fastq.gz /workdir/cystmetagenome_all/SScystsfall_R2.fastq.gz > SRfSSfallprok_aligned.sam -t 40
samtools fixmate -O bam SRfSSfallprok_aligned.sam SRfSSfallprok_fixmate.bam
samtools sort -o SRfSSfallprok_sorted.bam -O bam -T temp SRfSSfallprok_fixmate.bam
samtools index SRfSSfallprok_sorted.bam
anvi-init-bam SRfSSfallprok_sorted.bam -o SRfSSfallprok.bam

anvi-setup-scg-taxonomy
anvi-run-scg-taxonomy -c S3midprok.db --num-threads 40
anvi-run-scg-taxonomy -c SAmidprok.db --num-threads 40
anvi-run-scg-taxonomy -c SRmidprok.db --num-threads 40
anvi-run-scg-taxonomy -c SSmidprok.db --num-threads 40
anvi-run-scg-taxonomy -c S3fallprok.db --num-threads 40
anvi-run-scg-taxonomy -c SAfallprok.db --num-threads 40
anvi-run-scg-taxonomy -c SRfallprok.db --num-threads 40
anvi-run-scg-taxonomy -c SSfallprok.db --num-threads 40


#### S3 mid mapped to itself, and everyother sample. 
anvi-profile -i S3midprok.bam -c S3midprok.db --cluster-contigs --num-threads 40 --min-contig-length 2500 --output-dir S3midprok

anvi-profile -i S3mSAmidprok.bam -c S3midprok.db --cluster-contigs --num-threads 40 --min-contig-length 2500 --output-dir S3mSAmidprok

anvi-profile -i S3mSRmidprok.bam -c S3midprok.db --cluster-contigs --num-threads 40 --min-contig-length 2500 --output-dir S3mSRmidprok

anvi-profile -i S3mSSmidprok.bam -c S3midprok.db --cluster-contigs --num-threads 40 --min-contig-length 2500 --output-dir S3mSSmidprok

anvi-profile -i S3mS3fallprok.bam -c S3midprok.db --cluster-contigs --num-threads 40 --min-contig-length 2500 --output-dir S3mS3fallprok

anvi-profile -i S3mSAfallprok.bam -c S3midprok.db --cluster-contigs --num-threads 40 --min-contig-length 2500 --output-dir S3mSAfallprok

anvi-profile -i S3mSRfallprok.bam -c S3midprok.db --cluster-contigs --num-threads 40 --min-contig-length 2500 --output-dir S3mSRfallprok

anvi-profile -i S3mSSfallprok.bam -c S3midprok.db --cluster-contigs --num-threads 40 --min-contig-length 2500 --output-dir S3mSSfallprok

anvi-merge S3midprok/PROFILE.db S3mSAmidprok/PROFILE.db S3mSRmidprok/PROFILE.db S3mSSmidprok/PROFILE.db S3mS3fallprok/PROFILE.db S3mSAfallprok/PROFILE.db S3mSRfallprok/PROFILE.db S3mSSfallprok/PROFILE.db -o mergeS3midcontigs -c S3midprok.db

#### SA mid mapped to itself and everyother sample
anvi-profile -i SAmidprok.bam -c SAmidprok.db --cluster-contigs --num-threads 40 --min-contig-length 2500 --output-dir SAmidprok

anvi-profile -i SAmS3midprok.bam -c SAmidprok.db --cluster-contigs --num-threads 40 --min-contig-length 2500 --output-dir SAmS3midprok

anvi-profile -i SAmSRmidprok.bam -c SAmidprok.db --cluster-contigs --num-threads 40 --min-contig-length 2500 --output-dir SAmSRmidprok

anvi-profile -i SAmSSmidprok.bam -c SAmidprok.db --cluster-contigs --num-threads 40 --min-contig-length 2500 --output-dir SAmSSmidprok

anvi-profile -i SAmS3fallprok.bam -c SAmidprok.db --cluster-contigs --num-threads 40 --min-contig-length 2500 --output-dir SAmS3fallprok

anvi-profile -i SAmSAfallprok.bam -c SAmidprok.db --cluster-contigs --num-threads 40 --min-contig-length 2500 --output-dir SAmSAfallprok

anvi-profile -i SAmSRfallprok.bam -c SAmidprok.db --cluster-contigs --num-threads 40 --min-contig-length 2500 --output-dir SAmSRfallprok

anvi-profile -i SAmSSfallprok.bam -c SAmidprok.db --cluster-contigs --num-threads 40 --min-contig-length 2500 --output-dir SAmSSfallprok

anvi-merge SAmidprok/PROFILE.db  SAmS3midprok/PROFILE.db  SAmSRmidprok/PROFILE.db  SAmSSmidprok/PROFILE.db  SAmS3fallprok/PROFILE.db SAmSAfallprok/PROFILE.db SAmSRfallprok/PROFILE.db SAmSSfallprok/PROFILE.db -o mergeSAmidcontigs -c SAmidprok.db

#### SR mid mapped to itself and every other sample
anvi-profile -i SRmidprok.bam -c SRmidprok.db --cluster-contigs --num-threads 40 --min-contig-length 2500 --output-dir SRmidprok

anvi-profile -i SRmS3midprok.bam -c SRmidprok.db --cluster-contigs --num-threads 40 --min-contig-length 2500 --output-dir SRmS3midprok

anvi-profile -i SRmSAmidprok.bam -c SRmidprok.db --cluster-contigs --num-threads 40 --min-contig-length 2500 --output-dir SRmSAmidprok

anvi-profile -i SRmSSmidprok.bam -c SRmidprok.db --cluster-contigs --num-threads 40 --min-contig-length 2500 --output-dir SRmSSmidprok

anvi-profile -i SRmS3fallprok.bam -c SRmidprok.db --cluster-contigs --num-threads 40 --min-contig-length 2500 --output-dir SRmS3fallprok

anvi-profile -i SRmSAfallprok.bam -c SRmidprok.db --cluster-contigs --num-threads 40 --min-contig-length 2500 --output-dir SRmSAfallprok

anvi-profile -i SRmSRfallprok.bam -c SRmidprok.db --cluster-contigs --num-threads 40 --min-contig-length 2500 --output-dir SRmSRfallprok

anvi-profile -i SRmSSfallprok.bam -c SRmidprok.db --cluster-contigs --num-threads 40 --min-contig-length 2500 --output-dir SRmSSfallprok

anvi-merge SRmidprok/PROFILE.db  SRmS3midprok/PROFILE.db SRmSAmidprok/PROFILE.db SRmSSmidprok/PROFILE.db SRmS3fallprok/PROFILE.db SRmSAfallprok/PROFILE.db SRmSRfallprok/PROFILE.db SRmSSfallprok/PROFILE.db -o mergeSRmidcontigs -c SRmidprok.db

#### SS mid mapped to itself and every other sample
anvi-profile -i SSmidprok.bam -c SSmidprok.db --cluster-contigs --num-threads 40 --min-contig-length 2500 --output-dir SSmidprok

anvi-profile -i SSmS3midprok.bam -c SSmidprok.db --cluster-contigs --num-threads 40 --min-contig-length 2500 --output-dir SSmS3midprok

anvi-profile -i SSmSAmidprok.bam -c SSmidprok.db --cluster-contigs --num-threads 40 --min-contig-length 2500 --output-dir SSmSAmidprok

anvi-profile -i SSmSRmidprok.bam -c SSmidprok.db --cluster-contigs --num-threads 40 --min-contig-length 2500 --output-dir SSmSRmidprok

anvi-profile -i SSmS3fallprok.bam -c SSmidprok.db --cluster-contigs --num-threads 40 --min-contig-length 2500 --output-dir SSmS3fallprok

anvi-profile -i SSmSAfallprok.bam -c SSmidprok.db --cluster-contigs --num-threads 40 --min-contig-length 2500 --output-dir SSmSAfallprok

anvi-profile -i SSmSRfallprok.bam -c SSmidprok.db --cluster-contigs --num-threads 40 --min-contig-length 2500 --output-dir SSmSRfallprok

anvi-profile -i SSmSSfallprok.bam -c SSmidprok.db --cluster-contigs --num-threads 40 --min-contig-length 2500 --output-dir SSmSSfallprok

anvi-merge SSmidprok/PROFILE.db SSmS3midprok/PROFILE.db SSmSAmidprok/PROFILE.db SSmSRmidprok/PROFILE.db SSmS3fallprok/PROFILE.db SSmSAfallprok/PROFILE.db SSmSRfallprok/PROFILE.db SSmSSfallprok/PROFILE.db -o mergeSSmidcontigs -c SSmidprok.db

#### S3 fall mapped to itself and every other sample
anvi-profile -i S3fallprok.bam -c S3fallprok.db --cluster-contigs --num-threads 40 --min-contig-length 2500 --output-dir S3fallprok

anvi-profile -i S3fS3midprok.bam -c S3fallprok.db --cluster-contigs --num-threads 40 --min-contig-length 2500 --output-dir S3fS3midprok

anvi-profile -i S3fSAmidprok.bam -c S3fallprok.db --cluster-contigs --num-threads 40 --min-contig-length 2500 --output-dir S3fSAmidprok

anvi-profile -i S3fSRmidprok.bam -c S3fallprok.db --cluster-contigs --num-threads 40 --min-contig-length 2500 --output-dir S3fSRmidprok

anvi-profile -i S3fSSmidprok.bam -c S3fallprok.db --cluster-contigs --num-threads 40 --min-contig-length 2500 --output-dir S3fSSmidprok

anvi-profile -i S3fSAfallprok.bam -c S3fallprok.db --cluster-contigs --num-threads 40 --min-contig-length 2500 --output-dir S3fSAfallprok

anvi-profile -i S3fSRfallprok.bam -c S3fallprok.db --cluster-contigs --num-threads 40 --min-contig-length 2500 --output-dir S3fSRfallprok

anvi-profile -i S3fSSfallprok.bam -c S3fallprok.db --cluster-contigs --num-threads 40 --min-contig-length 2500 --output-dir S3fSSfallprok

anvi-merge S3fallprok/PROFILE.db S3fS3midprok/PROFILE.db S3fSAmidprok/PROFILE.db S3fSRmidprok/PROFILE.db S3fSSmidprok/PROFILE.db S3fSAfallprok/PROFILE.db S3fSRfallprok/PROFILE.db S3fSSfallprok/PROFILE.db -o mergeS3fallcontigs -c S3fallprok.db

#### SA fall mapped to itself and every other sample
anvi-profile -i SAfallprok.bam -c SAfallprok.db --cluster-contigs --num-threads 40 --min-contig-length 2500 --output-dir SAfallprok

anvi-profile -i SAfS3midprok.bam -c SAfallprok.db --cluster-contigs --num-threads 40 --min-contig-length 2500 --output-dir SAfS3midprok

anvi-profile -i SAfSAmidprok.bam -c SAfallprok.db --cluster-contigs --num-threads 40 --min-contig-length 2500 --output-dir SAfSAmidprok

anvi-profile -i SAfSRmidprok.bam -c SAfallprok.db --cluster-contigs --num-threads 40 --min-contig-length 2500 --output-dir SAfSRmidprok

anvi-profile -i SAfSSmidprok.bam -c SAfallprok.db --cluster-contigs --num-threads 40 --min-contig-length 2500 --output-dir SAfSSmidprok

anvi-profile -i SAfS3fallprok.bam -c SAfallprok.db --cluster-contigs --num-threads 40 --min-contig-length 2500 --output-dir SAfS3fallprok

anvi-profile -i SAfSRfallprok.bam -c SAfallprok.db --cluster-contigs --num-threads 40 --min-contig-length 2500 --output-dir SAfSRfallprok

anvi-profile -i SAfSSfallprok.bam -c SAfallprok.db --cluster-contigs --num-threads 40 --min-contig-length 2500 --output-dir SAfSSfallprok

anvi-merge SAfallprok/PROFILE.db SAfS3midprok/PROFILE.db SAfSAmidprok/PROFILE.db SAfSRmidprok/PROFILE.db SAfSSmidprok/PROFILE.db SAfS3fallprok/PROFILE.db SAfSRfallprok/PROFILE.db SAfSSfallprok/PROFILE.db -o mergeSAfallcontigs -c SAfallprok.db 

#### SR fall mapped to itself and every other sample
anvi-profile -i SRfallprok.bam -c SRfallprok.db --cluster-contigs --num-threads 40 --min-contig-length 2500 --output-dir SRfallprok

anvi-profile -i SRfS3midprok.bam -c SRfallprok.db --cluster-contigs --num-threads 40 --min-contig-length 2500 --output-dir SRfS3midprok

anvi-profile -i SRfSAmidprok.bam -c SRfallprok.db --cluster-contigs --num-threads 40 --min-contig-length 2500 --output-dir SRfSAmidprok

anvi-profile -i SRfSRmidprok.bam -c SRfallprok.db --cluster-contigs --num-threads 40 --min-contig-length 2500 --output-dir SRfSRmidprok

anvi-profile -i SRfSSmidprok.bam -c SRfallprok.db --cluster-contigs --num-threads 40 --min-contig-length 2500 --output-dir SRfSSmidprok

anvi-profile -i SRfS3fallprok.bam -c SRfallprok.db --cluster-contigs --num-threads 40 --min-contig-length 2500 --output-dir SRfS3fallprok

anvi-profile -i SRfSAfallprok.bam -c SRfallprok.db --cluster-contigs --num-threads 40 --min-contig-length 2500 --output-dir SRfSAfallprok

anvi-profile -i SRfSSfallprok.bam -c SRfallprok.db --cluster-contigs --num-threads 40 --min-contig-length 2500 --output-dir SRfSSfallprok

anvi-merge SRfallprok/PROFILE.db SRfS3midprok/PROFILE.db SRfSAmidprok/PROFILE.db SRfSRmidprok/PROFILE.db SRfSSmidprok/PROFILE.db SRfS3fallprok/PROFILE.db SRfSAfallprok/PROFILE.db SRfSSfallprok/PROFILE.db -o mergeSRfallcontigs -c SRfallprok.db

#### SS fall mapped to itself and every other sample
anvi-profile -i SSfallprok.bam -c SSfallprok.db --cluster-contigs --num-threads 40 --min-contig-length 2500 --output-dir SSfallprok

anvi-profile -i SSfS3midprok.bam -c SSfallprok.db --cluster-contigs --num-threads 40 --min-contig-length 2500 --output-dir SSfS3midprok

anvi-profile -i SSfSAmidprok.bam -c SSfallprok.db --cluster-contigs --num-threads 40 --min-contig-length 2500 --output-dir SSfSAmidprok

anvi-profile -i SSfSRmidprok.bam -c SSfallprok.db --cluster-contigs --num-threads 40 --min-contig-length 2500 --output-dir SSfSRmidprok

anvi-profile -i SSfSSmidprok.bam -c SSfallprok.db --cluster-contigs --num-threads 40 --min-contig-length 2500 --output-dir SSfSSmidprok

anvi-profile -i SSfS3fallprok.bam -c SSfallprok.db --cluster-contigs --num-threads 40 --min-contig-length 2500 --output-dir SSfS3fallprok

anvi-profile -i SSfSAfallprok.bam -c SSfallprok.db --cluster-contigs --num-threads 40 --min-contig-length 2500 --output-dir SSfSAfallprok

anvi-profile -i SSfSRfallprok.bam -c SSfallprok.db --cluster-contigs --num-threads 40 --min-contig-length 2500 --output-dir SSfSRfallprok

anvi-merge SSfallprok/PROFILE.db SSfS3midprok/PROFILE.db SSfSAmidprok/PROFILE.db SSfSRmidprok/PROFILE.db SSfSSmidprok/PROFILE.db SSfS3fallprok/PROFILE.db SSfSAfallprok/PROFILE.db SSfSRfallprok/PROFILE.db -o mergeSSfallcontigs -c SSfallprok.db


#### Export anvio split contigs
anvi-export-splits-and-coverages -p mergeS3midcontigs/PROFILE.db -c S3midprok.db
anvi-export-splits-and-coverages -p mergeSAmidcontigs/PROFILE.db -c SAmidprok.db
anvi-export-splits-and-coverages -p mergeSRmidcontigs/PROFILE.db -c SRmidprok.db
anvi-export-splits-and-coverages -p mergeSSmidcontigs/PROFILE.db -c SSmidprok.db
anvi-export-splits-and-coverages -p mergeS3fallcontigs/PROFILE.db -c S3fallprok.db
anvi-export-splits-and-coverages -p mergeSAfallcontigs/PROFILE.db -c SAfallprok.db
anvi-export-splits-and-coverages -p mergeSRfallcontigs/PROFILE.db -c SRfallprok.db
anvi-export-splits-and-coverages -p mergeSSfallcontigs/PROFILE.db -c SSfallprok.db

 
### Reciprical mapping with anvio split contigs to each set of reads. 
#### S3mid 
/programs/bwa-0.7.17/bwa index mergeS3midcontigs/mergeS3midcontigs-SPLITS.fa 

/programs/bwa-0.7.17/bwa mem mergeS3midcontigs/mergeS3midcontigs-SPLITS.fa /workdir/eag252/cystmetagenome_all/S3cystsmid_R1.fastq.gz /workdir/eag252/cystmetagenome_all/S3cystsmid_R2.fastq.gz > mergeS3midcontigs/mergeS3mid-split.sam -t 40

/programs/bwa-0.7.17/bwa mem mergeS3midcontigs/mergeS3midcontigs-SPLITS.fa /workdir/eag252/cystmetagenome_all/SAcystsmid_R1.fastq.gz /workdir/eag252/cystmetagenome_all/SAcystsmid_R2.fastq.gz > mergeS3midcontigs/mergeS3mSAmid-split.sam -t 40

/programs/bwa-0.7.17/bwa mem mergeS3midcontigs/mergeS3midcontigs-SPLITS.fa /workdir/eag252/cystmetagenome_all/SRcystsmid_R1.fastq.gz /workdir/eag252/cystmetagenome_all/SRcystsmid_R2.fastq.gz > mergeS3midcontigs/mergeS3mSRmid-split.sam -t 40

/programs/bwa-0.7.17/bwa mem mergeS3midcontigs/mergeS3midcontigs-SPLITS.fa /workdir/eag252/cystmetagenome_all/SScystsmid_R1.fastq.gz /workdir/eag252/cystmetagenome_all/SScystsmid_R2.fastq.gz > mergeS3midcontigs/mergeS3mSSmid-split.sam -t 40

/programs/bwa-0.7.17/bwa mem mergeS3midcontigs/mergeS3midcontigs-SPLITS.fa /workdir/eag252/cystmetagenome_all/S3cystsfall_R1.fastq.gz /workdir/eag252/cystmetagenome_all/S3cystsfall_R2.fastq.gz > mergeS3midcontigs/mergeS3mS3fall-split.sam -t 40

/programs/bwa-0.7.17/bwa mem mergeS3midcontigs/mergeS3midcontigs-SPLITS.fa /workdir/eag252/cystmetagenome_all/SAcystsfall_R1.fastq.gz /workdir/eag252/cystmetagenome_all/SAcystsfall_R2.fastq.gz > mergeS3midcontigs/mergeS3mSAfall-split.sam -t 40

/programs/bwa-0.7.17/bwa mem mergeS3midcontigs/mergeS3midcontigs-SPLITS.fa /workdir/eag252/cystmetagenome_all/SRcystsfall_R1.fastq.gz /workdir/eag252/cystmetagenome_all/SRcystsfall_R2.fastq.gz > mergeS3midcontigs/mergeS3mSRfall-split.sam -t 40

/programs/bwa-0.7.17/bwa mem mergeS3midcontigs/mergeS3midcontigs-SPLITS.fa /workdir/eag252/cystmetagenome_all/SScystsfall_R1.fastq.gz /workdir/eag252/cystmetagenome_all/SScystsfall_R2.fastq.gz > mergeS3midcontigs/mergeS3mSSfall-split.sam -t 40

/programs/samtools-1.15.1-r/bin/samtools fixmate -O bam mergeS3midcontigs/mergeS3mid-split.sam mergeS3midcontigs/mergeS3mid-split.fixmate.bam
/programs/samtools-1.15.1-r/bin/samtools fixmate -O bam mergeS3midcontigs/mergeS3mSAmid-split.sam mergeS3midcontigs/mergeS3mSAmid-split.fixmate.bam
/programs/samtools-1.15.1-r/bin/samtools fixmate -O bam mergeS3midcontigs/mergeS3mSRmid-split.sam mergeS3midcontigs/mergeS3mSRmid-split.fixmate.bam
/programs/samtools-1.15.1-r/bin/samtools fixmate -O bam mergeS3midcontigs/mergeS3mSSmid-split.sam mergeS3midcontigs/mergeS3mSSmid-split.fixmate.bam
/programs/samtools-1.15.1-r/bin/samtools fixmate -O bam mergeS3midcontigs/mergeS3mS3fall-split.sam mergeS3midcontigs/mergeS3mS3fall-split.fixmate.bam
/programs/samtools-1.15.1-r/bin/samtools fixmate -O bam mergeS3midcontigs/mergeS3mSAfall-split.sam mergeS3midcontigs/mergeS3mSAfall-split.fixmate.bam
/programs/samtools-1.15.1-r/bin/samtools fixmate -O bam mergeS3midcontigs/mergeS3mSRfall-split.sam mergeS3midcontigs/mergeS3mSRfall-split.fixmate.bam
/programs/samtools-1.15.1-r/bin/samtools fixmate -O bam mergeS3midcontigs/mergeS3mSSfall-split.sam mergeS3midcontigs/mergeS3mSSfall-split.fixmate.bam

/programs/samtools-1.15.1-r/bin/samtools sort -o mergeS3midcontigs/mergeS3mid-split.sorted.bam -O bam -T temp mergeS3midcontigs/mergeS3mid-split.fixmate.bam
/programs/samtools-1.15.1-r/bin/samtools sort -o mergeS3midcontigs/mergeS3mSAmid-split.sorted.bam -O bam -T temp mergeS3midcontigs/mergeS3mSAmid-split.fixmate.bam
/programs/samtools-1.15.1-r/bin/samtools sort -o mergeS3midcontigs/mergeS3mSRmid-split.sorted.bam -O bam -T temp mergeS3midcontigs/mergeS3mSRmid-split.fixmate.bam
/programs/samtools-1.15.1-r/bin/samtools sort -o mergeS3midcontigs/mergeS3mSSmid-split.sorted.bam -O bam -T temp mergeS3midcontigs/mergeS3mSSmid-split.fixmate.bam
/programs/samtools-1.15.1-r/bin/samtools sort -o mergeS3midcontigs/mergeS3mS3fall-split.sorted.bam -O bam -T temp mergeS3midcontigs/mergeS3mS3fall-split.fixmate.bam
/programs/samtools-1.15.1-r/bin/samtools sort -o mergeS3midcontigs/mergeS3mSAfall-split.sorted.bam -O bam -T temp mergeS3midcontigs/mergeS3mSAfall-split.fixmate.bam
/programs/samtools-1.15.1-r/bin/samtools sort -o mergeS3midcontigs/mergeS3mSRfall-split.sorted.bam -O bam -T temp mergeS3midcontigs/mergeS3mSRfall-split.fixmate.bam
/programs/samtools-1.15.1-r/bin/samtools sort -o mergeS3midcontigs/mergeS3mSSfall-split.sorted.bam -O bam -T temp mergeS3midcontigs/mergeS3mSSfall-split.fixmate.bam

/programs/samtools-1.15.1-r/bin/samtools index mergeS3midcontigs/mergeS3mid-split.sorted.bam
/programs/samtools-1.15.1-r/bin/samtools index mergeS3midcontigs/mergeS3mSAmid-split.sorted.bam
/programs/samtools-1.15.1-r/bin/samtools index mergeS3midcontigs/mergeS3mSRmid-split.sorted.bam
/programs/samtools-1.15.1-r/bin/samtools index mergeS3midcontigs/mergeS3mSSmid-split.sorted.bam
/programs/samtools-1.15.1-r/bin/samtools index mergeS3midcontigs/mergeS3mS3fall-split.sorted.bam
/programs/samtools-1.15.1-r/bin/samtools index mergeS3midcontigs/mergeS3mSAfall-split.sorted.bam
/programs/samtools-1.15.1-r/bin/samtools index mergeS3midcontigs/mergeS3mSRfall-split.sorted.bam
/programs/samtools-1.15.1-r/bin/samtools index mergeS3midcontigs/mergeS3mSSfall-split.sorted.bam


#### SA mid
/programs/bwa-0.7.17/bwa index mergeSAmidcontigs/mergeSAmidcontigs-SPLITS.fa 

/programs/bwa-0.7.17/bwa mem mergeSAmidcontigs/mergeSAmidcontigs-SPLITS.fa /workdir/eag252/cystmetagenome_all/SAcystsmid_R1.fastq.gz /workdir/eag252/cystmetagenome_all/SAcystsmid_R2.fastq.gz > mergeSAmidcontigs/mergeSAmid-split.sam -t 40
/programs/bwa-0.7.17/bwa mem mergeSAmidcontigs/mergeSAmidcontigs-SPLITS.fa /workdir/eag252/cystmetagenome_all/S3cystsmid_R1.fastq.gz /workdir/eag252/cystmetagenome_all/S3cystsmid_R2.fastq.gz > mergeSAmidcontigs/mergeSAmS3mid-split.sam -t 40
/programs/bwa-0.7.17/bwa mem mergeSAmidcontigs/mergeSAmidcontigs-SPLITS.fa /workdir/eag252/cystmetagenome_all/SRcystsmid_R1.fastq.gz /workdir/eag252/cystmetagenome_all/SRcystsmid_R2.fastq.gz > mergeSAmidcontigs/mergeSAmSRmid-split.sam -t 40
/programs/bwa-0.7.17/bwa mem mergeSAmidcontigs/mergeSAmidcontigs-SPLITS.fa /workdir/eag252/cystmetagenome_all/SScystsmid_R1.fastq.gz /workdir/eag252/cystmetagenome_all/SScystsmid_R2.fastq.gz > mergeSAmidcontigs/mergeSAmSSmid-split.sam -t 40
/programs/bwa-0.7.17/bwa mem mergeSAmidcontigs/mergeSAmidcontigs-SPLITS.fa /workdir/eag252/cystmetagenome_all/S3cystsfall_R1.fastq.gz /workdir/eag252/cystmetagenome_all/S3cystsfall_R2.fastq.gz > mergeSAmidcontigs/mergeSAmS3fall-split.sam -t 40
/programs/bwa-0.7.17/bwa mem mergeSAmidcontigs/mergeSAmidcontigs-SPLITS.fa /workdir/eag252/cystmetagenome_all/SAcystsfall_R1.fastq.gz /workdir/eag252/cystmetagenome_all/SAcystsfall_R2.fastq.gz > mergeSAmidcontigs/mergeSAmSAfall-split.sam -t 40
/programs/bwa-0.7.17/bwa mem mergeSAmidcontigs/mergeSAmidcontigs-SPLITS.fa /workdir/eag252/cystmetagenome_all/SRcystsfall_R1.fastq.gz /workdir/eag252/cystmetagenome_all/SRcystsfall_R2.fastq.gz > mergeSAmidcontigs/mergeSAmSRfall-split.sam -t 40
/programs/bwa-0.7.17/bwa mem mergeSAmidcontigs/mergeSAmidcontigs-SPLITS.fa /workdir/eag252/cystmetagenome_all/SScystsfall_R1.fastq.gz /workdir/eag252/cystmetagenome_all/SScystsfall_R2.fastq.gz > mergeSAmidcontigs/mergeSAmSSfall-split.sam -t 40

/programs/samtools-1.15.1-r/bin/samtools fixmate -O bam mergeSAmidcontigs/mergeSAmid-split.sam mergeSAmidcontigs/mergeSAmid-split.fixmate.bam
/programs/samtools-1.15.1-r/bin/samtools fixmate -O bam mergeSAmidcontigs/mergeSAmS3mid-split.sam mergeSAmidcontigs/mergeSAmS3mid-split.fixmate.bam
/programs/samtools-1.15.1-r/bin/samtools fixmate -O bam mergeSAmidcontigs/mergeSAmSRmid-split.sam mergeSAmidcontigs/mergeSAmSRmid-split.fixmate.bam
/programs/samtools-1.15.1-r/bin/samtools fixmate -O bam mergeSAmidcontigs/mergeSAmSSmid-split.sam mergeSAmidcontigs/mergeSAmSSmid-split.fixmate.bam
/programs/samtools-1.15.1-r/bin/samtools fixmate -O bam mergeSAmidcontigs/mergeSAmS3fall-split.sam mergeSAmidcontigs/mergeSAmS3fall-split.fixmate.bam
/programs/samtools-1.15.1-r/bin/samtools fixmate -O bam mergeSAmidcontigs/mergeSAmSAfall-split.sam mergeSAmidcontigs/mergeSAmSAfall-split.fixmate.bam
/programs/samtools-1.15.1-r/bin/samtools fixmate -O bam mergeSAmidcontigs/mergeSAmSRfall-split.sam mergeSAmidcontigs/mergeSAmSRfall-split.fixmate.bam
/programs/samtools-1.15.1-r/bin/samtools fixmate -O bam mergeSAmidcontigs/mergeSAmSSfall-split.sam mergeSAmidcontigs/mergeSAmSSfall-split.fixmate.bam

/programs/samtools-1.15.1-r/bin/samtools sort -o mergeSAmidcontigs/mergeSAmid-split.sorted.bam -O bam -T temp mergeSAmidcontigs/mergeSAmid-split.fixmate.bam
/programs/samtools-1.15.1-r/bin/samtools sort -o mergeSAmidcontigs/mergeSAmS3mid-split.sorted.bam -O bam -T temp mergeSAmidcontigs/mergeSAmS3mid-split.fixmate.bam
/programs/samtools-1.15.1-r/bin/samtools sort -o mergeSAmidcontigs/mergeSAmSRmid-split.sorted.bam -O bam -T temp mergeSAmidcontigs/mergeSAmSRmid-split.fixmate.bam
/programs/samtools-1.15.1-r/bin/samtools sort -o mergeSAmidcontigs/mergeSAmSSmid-split.sorted.bam -O bam -T temp mergeSAmidcontigs/mergeSAmSSmid-split.fixmate.bam
/programs/samtools-1.15.1-r/bin/samtools sort -o mergeSAmidcontigs/mergeSAmS3fall-split.sorted.bam -O bam -T temp mergeSAmidcontigs/mergeSAmS3fall-split.fixmate.bam
/programs/samtools-1.15.1-r/bin/samtools sort -o mergeSAmidcontigs/mergeSAmSAfall-split.sorted.bam -O bam -T temp mergeSAmidcontigs/mergeSAmSAfall-split.fixmate.bam
/programs/samtools-1.15.1-r/bin/samtools sort -o mergeSAmidcontigs/mergeSAmSRfall-split.sorted.bam -O bam -T temp mergeSAmidcontigs/mergeSAmSRfall-split.fixmate.bam
/programs/samtools-1.15.1-r/bin/samtools sort -o mergeSAmidcontigs/mergeSAmSSfall-split.sorted.bam -O bam -T temp mergeSAmidcontigs/mergeSAmSSfall-split.fixmate.bam

/programs/samtools-1.15.1-r/bin/samtools index mergeSAmidcontigs/mergeSAmid-split.sorted.bam
/programs/samtools-1.15.1-r/bin/samtools index mergeSAmidcontigs/mergeSAmS3mid-split.sorted.bam
/programs/samtools-1.15.1-r/bin/samtools index mergeSAmidcontigs/mergeSAmSRmid-split.sorted.bam
/programs/samtools-1.15.1-r/bin/samtools index mergeSAmidcontigs/mergeSAmSSmid-split.sorted.bam
/programs/samtools-1.15.1-r/bin/samtools index mergeSAmidcontigs/mergeSAmS3fall-split.sorted.bam
/programs/samtools-1.15.1-r/bin/samtools index mergeSAmidcontigs/mergeSAmSAfall-split.sorted.bam
/programs/samtools-1.15.1-r/bin/samtools index mergeSAmidcontigs/mergeSAmSRfall-split.sorted.bam
/programs/samtools-1.15.1-r/bin/samtools index mergeSAmidcontigs/mergeSAmSSfall-split.sorted.bam


#### SRmid
/programs/bwa-0.7.17/bwa index mergeSRmidcontigs/mergeSRmidcontigs-SPLITS.fa 

/programs/bwa-0.7.17/bwa mem mergeSRmidcontigs/mergeSRmidcontigs-SPLITS.fa /workdir/eag252/cystmetagenome_all/SRcystsmid_R1.fastq.gz /workdir/eag252/cystmetagenome_all/SRcystsmid_R2.fastq.gz > mergeSRmidcontigs/mergeSRmid-split.sam -t 40
/programs/bwa-0.7.17/bwa mem mergeSRmidcontigs/mergeSRmidcontigs-SPLITS.fa /workdir/eag252/cystmetagenome_all/S3cystsmid_R1.fastq.gz /workdir/eag252/cystmetagenome_all/S3cystsmid_R2.fastq.gz > mergeSRmidcontigs/mergeSRmS3mid-split.sam -t 40
/programs/bwa-0.7.17/bwa mem mergeSRmidcontigs/mergeSRmidcontigs-SPLITS.fa /workdir/eag252/cystmetagenome_all/SAcystsmid_R1.fastq.gz /workdir/eag252/cystmetagenome_all/SAcystsmid_R2.fastq.gz > mergeSRmidcontigs/mergeSRmSAmid-split.sam -t 40
/programs/bwa-0.7.17/bwa mem mergeSRmidcontigs/mergeSRmidcontigs-SPLITS.fa /workdir/eag252/cystmetagenome_all/SScystsmid_R1.fastq.gz /workdir/eag252/cystmetagenome_all/SScystsmid_R2.fastq.gz > mergeSRmidcontigs/mergeSRmSSmid-split.sam -t 40
/programs/bwa-0.7.17/bwa mem mergeSRmidcontigs/mergeSRmidcontigs-SPLITS.fa /workdir/eag252/cystmetagenome_all/S3cystsfall_R1.fastq.gz /workdir/eag252/cystmetagenome_all/S3cystsfall_R2.fastq.gz > mergeSRmidcontigs/mergeSRmS3fall-split.sam -t 40
/programs/bwa-0.7.17/bwa mem mergeSRmidcontigs/mergeSRmidcontigs-SPLITS.fa /workdir/eag252/cystmetagenome_all/SAcystsfall_R1.fastq.gz /workdir/eag252/cystmetagenome_all/SAcystsfall_R2.fastq.gz > mergeSRmidcontigs/mergeSRmSAfall-split.sam -t 40
/programs/bwa-0.7.17/bwa mem mergeSRmidcontigs/mergeSRmidcontigs-SPLITS.fa /workdir/eag252/cystmetagenome_all/SRcystsfall_R1.fastq.gz /workdir/eag252/cystmetagenome_all/SRcystsfall_R2.fastq.gz > mergeSRmidcontigs/mergeSRmSRfall-split.sam -t 40
/programs/bwa-0.7.17/bwa mem mergeSRmidcontigs/mergeSRmidcontigs-SPLITS.fa /workdir/eag252/cystmetagenome_all/SScystsfall_R1.fastq.gz /workdir/eag252/cystmetagenome_all/SScystsfall_R2.fastq.gz > mergeSRmidcontigs/mergeSRmSSfall-split.sam -t 40

/programs/samtools-1.15.1-r/bin/samtools fixmate -O bam mergeSRmidcontigs/mergeSRmid-split.sam mergeSRmidcontigs/mergeSRmid-split.fixmate.bam
/programs/samtools-1.15.1-r/bin/samtools fixmate -O bam mergeSRmidcontigs/mergeSRmS3mid-split.sam mergeSRmidcontigs/mergeSRmS3mid-split.fixmate.bam
/programs/samtools-1.15.1-r/bin/samtools fixmate -O bam mergeSRmidcontigs/mergeSRmSAmid-split.sam mergeSRmidcontigs/mergeSRmSAmid-split.fixmate.bam
/programs/samtools-1.15.1-r/bin/samtools fixmate -O bam mergeSRmidcontigs/mergeSRmSSmid-split.sam mergeSRmidcontigs/mergeSRmSSmid-split.fixmate.bam
/programs/samtools-1.15.1-r/bin/samtools fixmate -O bam mergeSRmidcontigs/mergeSRmS3fall-split.sam mergeSRmidcontigs/mergeSRmS3fall-split.fixmate.bam
/programs/samtools-1.15.1-r/bin/samtools fixmate -O bam mergeSRmidcontigs/mergeSRmSAfall-split.sam mergeSRmidcontigs/mergeSRmSAfall-split.fixmate.bam
/programs/samtools-1.15.1-r/bin/samtools fixmate -O bam mergeSRmidcontigs/mergeSRmSRfall-split.sam mergeSRmidcontigs/mergeSRmSRfall-split.fixmate.bam
/programs/samtools-1.15.1-r/bin/samtools fixmate -O bam mergeSRmidcontigs/mergeSRmSSfall-split.sam mergeSRmidcontigs/mergeSRmSSfall-split.fixmate.bam

/programs/samtools-1.15.1-r/bin/samtools sort -o mergeSRmidcontigs/mergeSRmid-split.sorted.bam -O bam -T temp mergeSRmidcontigs/mergeSRmid-split.fixmate.bam
/programs/samtools-1.15.1-r/bin/samtools sort -o mergeSRmidcontigs/mergeSRmS3mid-split.sorted.bam -O bam -T temp mergeSRmidcontigs/mergeSRmS3mid-split.fixmate.bam
/programs/samtools-1.15.1-r/bin/samtools sort -o mergeSRmidcontigs/mergeSRmSAmid-split.sorted.bam -O bam -T temp mergeSRmidcontigs/mergeSRmSAmid-split.fixmate.bam
/programs/samtools-1.15.1-r/bin/samtools sort -o mergeSRmidcontigs/mergeSRmSSmid-split.sorted.bam -O bam -T temp mergeSRmidcontigs/mergeSRmSSmid-split.fixmate.bam
/programs/samtools-1.15.1-r/bin/samtools sort -o mergeSRmidcontigs/mergeSRmS3fall-split.sorted.bam -O bam -T temp mergeSRmidcontigs/mergeSRmS3fall-split.fixmate.bam
/programs/samtools-1.15.1-r/bin/samtools sort -o mergeSRmidcontigs/mergeSRmSAfall-split.sorted.bam -O bam -T temp mergeSRmidcontigs/mergeSRmSAfall-split.fixmate.bam
/programs/samtools-1.15.1-r/bin/samtools sort -o mergeSRmidcontigs/mergeSRmSRfall-split.sorted.bam -O bam -T temp mergeSRmidcontigs/mergeSRmSRfall-split.fixmate.bam
/programs/samtools-1.15.1-r/bin/samtools sort -o mergeSRmidcontigs/mergeSRmSSfall-split.sorted.bam -O bam -T temp mergeSRmidcontigs/mergeSRmSSfall-split.fixmate.bam

/programs/samtools-1.15.1-r/bin/samtools index mergeSRmidcontigs/mergeSRmid-split.sorted.bam
/programs/samtools-1.15.1-r/bin/samtools index mergeSRmidcontigs/mergeSRmS3mid-split.sorted.bam
/programs/samtools-1.15.1-r/bin/samtools index mergeSRmidcontigs/mergeSRmSAmid-split.sorted.bam
/programs/samtools-1.15.1-r/bin/samtools index mergeSRmidcontigs/mergeSRmSSmid-split.sorted.bam
/programs/samtools-1.15.1-r/bin/samtools index mergeSRmidcontigs/mergeSRmS3fall-split.sorted.bam
/programs/samtools-1.15.1-r/bin/samtools index mergeSRmidcontigs/mergeSRmSAfall-split.sorted.bam
/programs/samtools-1.15.1-r/bin/samtools index mergeSRmidcontigs/mergeSRmSRfall-split.sorted.bam
/programs/samtools-1.15.1-r/bin/samtools index mergeSRmidcontigs/mergeSRmSSfall-split.sorted.bam


#### SSmid
/programs/bwa-0.7.17/bwa index mergeSSmidcontigs/mergeSSmidcontigs-SPLITS.fa 

/programs/bwa-0.7.17/bwa mem mergeSSmidcontigs/mergeSSmidcontigs-SPLITS.fa /workdir/eag252/cystmetagenome_all/SScystsmid_R1.fastq.gz /workdir/eag252/cystmetagenome_all/SScystsmid_R2.fastq.gz > mergeSSmidcontigs/mergeSSmid-split.sam -t 40
/programs/bwa-0.7.17/bwa mem mergeSSmidcontigs/mergeSSmidcontigs-SPLITS.fa /workdir/eag252/cystmetagenome_all/S3cystsmid_R1.fastq.gz /workdir/eag252/cystmetagenome_all/S3cystsmid_R2.fastq.gz > mergeSSmidcontigs/mergeSSmS3mid-split.sam -t 40
/programs/bwa-0.7.17/bwa mem mergeSSmidcontigs/mergeSSmidcontigs-SPLITS.fa /workdir/eag252/cystmetagenome_all/SAcystsmid_R1.fastq.gz /workdir/eag252/cystmetagenome_all/SAcystsmid_R2.fastq.gz > mergeSSmidcontigs/mergeSSmSAmid-split.sam -t 40
/programs/bwa-0.7.17/bwa mem mergeSSmidcontigs/mergeSSmidcontigs-SPLITS.fa /workdir/eag252/cystmetagenome_all/SRcystsmid_R1.fastq.gz /workdir/eag252/cystmetagenome_all/SRcystsmid_R2.fastq.gz > mergeSSmidcontigs/mergeSSmSRmid-split.sam -t 40
/programs/bwa-0.7.17/bwa mem mergeSSmidcontigs/mergeSSmidcontigs-SPLITS.fa /workdir/eag252/cystmetagenome_all/S3cystsfall_R1.fastq.gz /workdir/eag252/cystmetagenome_all/S3cystsfall_R2.fastq.gz > mergeSSmidcontigs/mergeSSmS3fall-split.sam -t 40
/programs/bwa-0.7.17/bwa mem mergeSSmidcontigs/mergeSSmidcontigs-SPLITS.fa /workdir/eag252/cystmetagenome_all/SAcystsfall_R1.fastq.gz /workdir/eag252/cystmetagenome_all/SAcystsfall_R2.fastq.gz > mergeSSmidcontigs/mergeSSmSAfall-split.sam -t 40
/programs/bwa-0.7.17/bwa mem mergeSSmidcontigs/mergeSSmidcontigs-SPLITS.fa /workdir/eag252/cystmetagenome_all/SRcystsfall_R1.fastq.gz /workdir/eag252/cystmetagenome_all/SRcystsfall_R2.fastq.gz > mergeSSmidcontigs/mergeSSmSRfall-split.sam -t 40
/programs/bwa-0.7.17/bwa mem mergeSSmidcontigs/mergeSSmidcontigs-SPLITS.fa /workdir/eag252/cystmetagenome_all/SScystsfall_R1.fastq.gz /workdir/eag252/cystmetagenome_all/SScystsfall_R2.fastq.gz > mergeSSmidcontigs/mergeSSmSSfall-split.sam -t 40

/programs/samtools-1.15.1-r/bin/samtools fixmate -O bam mergeSSmidcontigs/mergeSSmid-split.sam mergeSSmidcontigs/mergeSSmid-split.fixmate.bam
/programs/samtools-1.15.1-r/bin/samtools fixmate -O bam mergeSSmidcontigs/mergeSSmS3mid-split.sam mergeSSmidcontigs/mergeSSmS3mid-split.fixmate.bam
/programs/samtools-1.15.1-r/bin/samtools fixmate -O bam mergeSSmidcontigs/mergeSSmSAmid-split.sam mergeSSmidcontigs/mergeSSmSAmid-split.fixmate.bam
/programs/samtools-1.15.1-r/bin/samtools fixmate -O bam mergeSSmidcontigs/mergeSSmSRmid-split.sam mergeSSmidcontigs/mergeSSmSRmid-split.fixmate.bam
/programs/samtools-1.15.1-r/bin/samtools fixmate -O bam mergeSSmidcontigs/mergeSSmS3fall-split.sam mergeSSmidcontigs/mergeSSmS3fall-split.fixmate.bam
/programs/samtools-1.15.1-r/bin/samtools fixmate -O bam mergeSSmidcontigs/mergeSSmSAfall-split.sam mergeSSmidcontigs/mergeSSmSAfall-split.fixmate.bam
/programs/samtools-1.15.1-r/bin/samtools fixmate -O bam mergeSSmidcontigs/mergeSSmSRfall-split.sam mergeSSmidcontigs/mergeSSmSRfall-split.fixmate.bam
/programs/samtools-1.15.1-r/bin/samtools fixmate -O bam mergeSSmidcontigs/mergeSSmSSfall-split.sam mergeSSmidcontigs/mergeSSmSSfall-split.fixmate.bam

/programs/samtools-1.15.1-r/bin/samtools sort -o mergeSSmidcontigs/mergeSSmid-split.sorted.bam -O bam -T temp mergeSSmidcontigs/mergeSSmid-split.fixmate.bam
/programs/samtools-1.15.1-r/bin/samtools sort -o mergeSSmidcontigs/mergeSSmS3mid-split.sorted.bam -O bam -T temp mergeSSmidcontigs/mergeSSmS3mid-split.fixmate.bam
/programs/samtools-1.15.1-r/bin/samtools sort -o mergeSSmidcontigs/mergeSSmSAmid-split.sorted.bam -O bam -T temp mergeSSmidcontigs/mergeSSmSAmid-split.fixmate.bam
/programs/samtools-1.15.1-r/bin/samtools sort -o mergeSSmidcontigs/mergeSSmSRmid-split.sorted.bam -O bam -T temp mergeSSmidcontigs/mergeSSmSRmid-split.fixmate.bam
/programs/samtools-1.15.1-r/bin/samtools sort -o mergeSSmidcontigs/mergeSSmS3fall-split.sorted.bam -O bam -T temp mergeSSmidcontigs/mergeSSmS3fall-split.fixmate.bam
/programs/samtools-1.15.1-r/bin/samtools sort -o mergeSSmidcontigs/mergeSSmSAfall-split.sorted.bam -O bam -T temp mergeSSmidcontigs/mergeSSmSAfall-split.fixmate.bam
/programs/samtools-1.15.1-r/bin/samtools sort -o mergeSSmidcontigs/mergeSSmSRfall-split.sorted.bam -O bam -T temp mergeSSmidcontigs/mergeSSmSRfall-split.fixmate.bam
/programs/samtools-1.15.1-r/bin/samtools sort -o mergeSSmidcontigs/mergeSSmSSfall-split.sorted.bam -O bam -T temp mergeSSmidcontigs/mergeSSmSSfall-split.fixmate.bam

/programs/samtools-1.15.1-r/bin/samtools index mergeSSmidcontigs/mergeSSmid-split.sorted.bam
/programs/samtools-1.15.1-r/bin/samtools index mergeSSmidcontigs/mergeSSmS3mid-split.sorted.bam
/programs/samtools-1.15.1-r/bin/samtools index mergeSSmidcontigs/mergeSSmSAmid-split.sorted.bam
/programs/samtools-1.15.1-r/bin/samtools index mergeSSmidcontigs/mergeSSmSRmid-split.sorted.bam
/programs/samtools-1.15.1-r/bin/samtools index mergeSSmidcontigs/mergeSSmS3fall-split.sorted.bam
/programs/samtools-1.15.1-r/bin/samtools index mergeSSmidcontigs/mergeSSmSAfall-split.sorted.bam
/programs/samtools-1.15.1-r/bin/samtools index mergeSSmidcontigs/mergeSSmSRfall-split.sorted.bam
/programs/samtools-1.15.1-r/bin/samtools index mergeSSmidcontigs/mergeSSmSSfall-split.sorted.bam

#### S3fall
/programs/bwa-0.7.17/bwa index mergeS3fallcontigs/mergeS3fallcontigs-SPLITS.fa 
/programs/bwa-0.7.17/bwa mem mergeS3fallcontigs/mergeS3fallcontigs-SPLITS.fa /workdir/eag252/cystmetagenome_all/S3cystsfall_R1.fastq.gz /workdir/eag252/cystmetagenome_all/S3cystsfall_R2.fastq.gz > mergeS3fallcontigs/mergeS3fall-split.sam -t 40
/programs/bwa-0.7.17/bwa mem mergeS3fallcontigs/mergeS3fallcontigs-SPLITS.fa /workdir/eag252/cystmetagenome_all/S3cystsmid_R1.fastq.gz /workdir/eag252/cystmetagenome_all/S3cystsmid_R2.fastq.gz > mergeS3fallcontigs/mergeS3fS3mid-split.sam -t 40
/programs/bwa-0.7.17/bwa mem mergeS3fallcontigs/mergeS3fallcontigs-SPLITS.fa /workdir/eag252/cystmetagenome_all/SAcystsmid_R1.fastq.gz /workdir/eag252/cystmetagenome_all/SAcystsmid_R2.fastq.gz > mergeS3fallcontigs/mergeS3fSAmid-split.sam -t 40
/programs/bwa-0.7.17/bwa mem mergeS3fallcontigs/mergeS3fallcontigs-SPLITS.fa /workdir/eag252/cystmetagenome_all/SRcystsmid_R1.fastq.gz /workdir/eag252/cystmetagenome_all/SRcystsmid_R2.fastq.gz > mergeS3fallcontigs/mergeS3fSRmid-split.sam -t 40
/programs/bwa-0.7.17/bwa mem mergeS3fallcontigs/mergeS3fallcontigs-SPLITS.fa /workdir/eag252/cystmetagenome_all/SScystsmid_R1.fastq.gz /workdir/eag252/cystmetagenome_all/SScystsmid_R2.fastq.gz > mergeS3fallcontigs/mergeS3fSSmid-split.sam -t 40
/programs/bwa-0.7.17/bwa mem mergeS3fallcontigs/mergeS3fallcontigs-SPLITS.fa /workdir/eag252/cystmetagenome_all/SAcystsfall_R1.fastq.gz /workdir/eag252/cystmetagenome_all/SAcystsfall_R2.fastq.gz > mergeS3fallcontigs/mergeS3fSAfall-split.sam -t 40
/programs/bwa-0.7.17/bwa mem mergeS3fallcontigs/mergeS3fallcontigs-SPLITS.fa /workdir/eag252/cystmetagenome_all/SRcystsfall_R1.fastq.gz /workdir/eag252/cystmetagenome_all/SRcystsfall_R2.fastq.gz > mergeS3fallcontigs/mergeS3fSRfall-split.sam -t 40
/programs/bwa-0.7.17/bwa mem mergeS3fallcontigs/mergeS3fallcontigs-SPLITS.fa /workdir/eag252/cystmetagenome_all/SScystsfall_R1.fastq.gz /workdir/eag252/cystmetagenome_all/SScystsfall_R2.fastq.gz > mergeS3fallcontigs/mergeS3fSSfall-split.sam -t 40

/programs/samtools-1.15.1-r/bin/samtools fixmate -O bam mergeS3fallcontigs/mergeS3fall-split.sam mergeS3fallcontigs/mergeS3fall-split.fixmate.bam
/programs/samtools-1.15.1-r/bin/samtools fixmate -O bam mergeS3fallcontigs/mergeS3fS3mid-split.sam mergeS3fallcontigs/mergeS3fS3mid-split.fixmate.bam
/programs/samtools-1.15.1-r/bin/samtools fixmate -O bam mergeS3fallcontigs/mergeS3fSAmid-split.sam mergeS3fallcontigs/mergeS3fSAmid-split.fixmate.bam
/programs/samtools-1.15.1-r/bin/samtools fixmate -O bam mergeS3fallcontigs/mergeS3fSRmid-split.sam mergeS3fallcontigs/mergeS3fSRmid-split.fixmate.bam
/programs/samtools-1.15.1-r/bin/samtools fixmate -O bam mergeS3fallcontigs/mergeS3fSSmid-split.sam mergeS3fallcontigs/mergeS3fSSmid-split.fixmate.bam
/programs/samtools-1.15.1-r/bin/samtools fixmate -O bam mergeS3fallcontigs/mergeS3fSAfall-split.sam mergeS3fallcontigs/mergeS3fSAfall-split.fixmate.bam
/programs/samtools-1.15.1-r/bin/samtools fixmate -O bam mergeS3fallcontigs/mergeS3fSRfall-split.sam mergeS3fallcontigs/mergeS3fSRfall-split.fixmate.bam
/programs/samtools-1.15.1-r/bin/samtools fixmate -O bam mergeS3fallcontigs/mergeS3fSSfall-split.sam mergeS3fallcontigs/mergeS3fSSfall-split.fixmate.bam

/programs/samtools-1.15.1-r/bin/samtools sort -o mergeS3fallcontigs/mergeS3fall-split.sorted.bam -O bam -T temp mergeS3fallcontigs/mergeS3fall-split.fixmate.bam
/programs/samtools-1.15.1-r/bin/samtools sort -o mergeS3fallcontigs/mergeS3fS3mid-split.sorted.bam -O bam -T temp mergeS3fallcontigs/mergeS3fS3mid-split.fixmate.bam
/programs/samtools-1.15.1-r/bin/samtools sort -o mergeS3fallcontigs/mergeS3fSAmid-split.sorted.bam -O bam -T temp mergeS3fallcontigs/mergeS3fSAmid-split.fixmate.bam
/programs/samtools-1.15.1-r/bin/samtools sort -o mergeS3fallcontigs/mergeS3fSRmid-split.sorted.bam -O bam -T temp mergeS3fallcontigs/mergeS3fSRmid-split.fixmate.bam
/programs/samtools-1.15.1-r/bin/samtools sort -o mergeS3fallcontigs/mergeS3fSSmid-split.sorted.bam -O bam -T temp mergeS3fallcontigs/mergeS3fSSmid-split.fixmate.bam
/programs/samtools-1.15.1-r/bin/samtools sort -o mergeS3fallcontigs/mergeS3fSAfall-split.sorted.bam -O bam -T temp mergeS3fallcontigs/mergeS3fSAfall-split.fixmate.bam
/programs/samtools-1.15.1-r/bin/samtools sort -o mergeS3fallcontigs/mergeS3fSRfall-split.sorted.bam -O bam -T temp mergeS3fallcontigs/mergeS3fSRfall-split.fixmate.bam
/programs/samtools-1.15.1-r/bin/samtools sort -o mergeS3fallcontigs/mergeS3fSSfall-split.sorted.bam -O bam -T temp mergeS3fallcontigs/mergeS3fSSfall-split.fixmate.bam

/programs/samtools-1.15.1-r/bin/samtools index mergeS3fallcontigs/mergeS3fall-split.sorted.bam
/programs/samtools-1.15.1-r/bin/samtools index mergeS3fallcontigs/mergeS3fS3mid-split.sorted.bam
/programs/samtools-1.15.1-r/bin/samtools index mergeS3fallcontigs/mergeS3fSAmid-split.sorted.bam
/programs/samtools-1.15.1-r/bin/samtools index mergeS3fallcontigs/mergeS3fSRmid-split.sorted.bam
/programs/samtools-1.15.1-r/bin/samtools index mergeS3fallcontigs/mergeS3fSSmid-split.sorted.bam
/programs/samtools-1.15.1-r/bin/samtools index mergeS3fallcontigs/mergeS3fSAfall-split.sorted.bam
/programs/samtools-1.15.1-r/bin/samtools index mergeS3fallcontigs/mergeS3fSRfall-split.sorted.bam
/programs/samtools-1.15.1-r/bin/samtools index mergeS3fallcontigs/mergeS3fSSfall-split.sorted.bam


#### SAfall
/programs/bwa-0.7.17/bwa index mergeSAfallcontigs/mergeSAfallcontigs-SPLITS.fa 

/programs/bwa-0.7.17/bwa mem mergeSAfallcontigs/mergeSAfallcontigs-SPLITS.fa /workdir/eag252/cystmetagenome_all/SAcystsfall_R1.fastq.gz /workdir/eag252/cystmetagenome_all/SAcystsfall_R2.fastq.gz > mergeSAfallcontigs/mergeSAfall-split.sam -t 40
/programs/bwa-0.7.17/bwa mem mergeSAfallcontigs/mergeSAfallcontigs-SPLITS.fa /workdir/eag252/cystmetagenome_all/S3cystsmid_R1.fastq.gz /workdir/eag252/cystmetagenome_all/S3cystsmid_R2.fastq.gz > mergeSAfallcontigs/mergeSAfS3mid-split.sam -t 40
/programs/bwa-0.7.17/bwa mem mergeSAfallcontigs/mergeSAfallcontigs-SPLITS.fa /workdir/eag252/cystmetagenome_all/SAcystsmid_R1.fastq.gz /workdir/eag252/cystmetagenome_all/SAcystsmid_R2.fastq.gz > mergeSAfallcontigs/mergeSAfSAmid-split.sam -t 40
/programs/bwa-0.7.17/bwa mem mergeSAfallcontigs/mergeSAfallcontigs-SPLITS.fa /workdir/eag252/cystmetagenome_all/SRcystsmid_R1.fastq.gz /workdir/eag252/cystmetagenome_all/SRcystsmid_R2.fastq.gz > mergeSAfallcontigs/mergeSAfSRmid-split.sam -t 40
/programs/bwa-0.7.17/bwa mem mergeSAfallcontigs/mergeSAfallcontigs-SPLITS.fa /workdir/eag252/cystmetagenome_all/SScystsmid_R1.fastq.gz /workdir/eag252/cystmetagenome_all/SScystsmid_R2.fastq.gz > mergeSAfallcontigs/mergeSAfSSmid-split.sam -t 40
/programs/bwa-0.7.17/bwa mem mergeSAfallcontigs/mergeSAfallcontigs-SPLITS.fa /workdir/eag252/cystmetagenome_all/S3cystsfall_R1.fastq.gz /workdir/eag252/cystmetagenome_all/S3cystsfall_R2.fastq.gz > mergeSAfallcontigs/mergeSAfS3fall-split.sam -t 40
/programs/bwa-0.7.17/bwa mem mergeSAfallcontigs/mergeSAfallcontigs-SPLITS.fa /workdir/eag252/cystmetagenome_all/SRcystsfall_R1.fastq.gz /workdir/eag252/cystmetagenome_all/SRcystsfall_R2.fastq.gz > mergeSAfallcontigs/mergeSAfSRfall-split.sam -t 40
/programs/bwa-0.7.17/bwa mem mergeSAfallcontigs/mergeSAfallcontigs-SPLITS.fa /workdir/eag252/cystmetagenome_all/SScystsfall_R1.fastq.gz /workdir/eag252/cystmetagenome_all/SScystsfall_R2.fastq.gz > mergeSAfallcontigs/mergeSAfSSfall-split.sam -t 40

/programs/samtools-1.15.1-r/bin/samtools fixmate -O bam mergeSAfallcontigs/mergeSAfall-split.sam mergeSAfallcontigs/mergeSAfall-split.fixmate.bam
/programs/samtools-1.15.1-r/bin/samtools fixmate -O bam mergeSAfallcontigs/mergeSAfS3mid-split.sam mergeSAfallcontigs/mergeSAfS3mid-split.fixmate.bam
/programs/samtools-1.15.1-r/bin/samtools fixmate -O bam mergeSAfallcontigs/mergeSAfSAmid-split.sam mergeSAfallcontigs/mergeSAfSAmid-split.fixmate.bam
/programs/samtools-1.15.1-r/bin/samtools fixmate -O bam mergeSAfallcontigs/mergeSAfSRmid-split.sam mergeSAfallcontigs/mergeSAfSRmid-split.fixmate.bam
/programs/samtools-1.15.1-r/bin/samtools fixmate -O bam mergeSAfallcontigs/mergeSAfSSmid-split.sam mergeSAfallcontigs/mergeSAfSSmid-split.fixmate.bam
/programs/samtools-1.15.1-r/bin/samtools fixmate -O bam mergeSAfallcontigs/mergeSAfS3fall-split.sam mergeSAfallcontigs/mergeSAfS3fall-split.fixmate.bam
/programs/samtools-1.15.1-r/bin/samtools fixmate -O bam mergeSAfallcontigs/mergeSAfSRfall-split.sam mergeSAfallcontigs/mergeSAfSRfall-split.fixmate.bam
/programs/samtools-1.15.1-r/bin/samtools fixmate -O bam mergeSAfallcontigs/mergeSAfSSfall-split.sam mergeSAfallcontigs/mergeSAfSSfall-split.fixmate.bam

/programs/samtools-1.15.1-r/bin/samtools sort -o mergeSAfallcontigs/mergeSAfall-split.sorted.bam -O bam -T temp mergeSAfallcontigs/mergeSAfall-split.fixmate.bam
/programs/samtools-1.15.1-r/bin/samtools sort -o mergeSAfallcontigs/mergeSAfS3mid-split.sorted.bam -O bam -T temp mergeSAfallcontigs/mergeSAfS3mid-split.fixmate.bam
/programs/samtools-1.15.1-r/bin/samtools sort -o mergeSAfallcontigs/mergeSAfSAmid-split.sorted.bam -O bam -T temp mergeSAfallcontigs/mergeSAfSAmid-split.fixmate.bam
/programs/samtools-1.15.1-r/bin/samtools sort -o mergeSAfallcontigs/mergeSAfSRmid-split.sorted.bam -O bam -T temp mergeSAfallcontigs/mergeSAfSRmid-split.fixmate.bam
/programs/samtools-1.15.1-r/bin/samtools sort -o mergeSAfallcontigs/mergeSAfSSmid-split.sorted.bam -O bam -T temp mergeSAfallcontigs/mergeSAfSSmid-split.fixmate.bam
/programs/samtools-1.15.1-r/bin/samtools sort -o mergeSAfallcontigs/mergeSAfS3fall-split.sorted.bam -O bam -T temp mergeSAfallcontigs/mergeSAfS3fall-split.fixmate.bam
/programs/samtools-1.15.1-r/bin/samtools sort -o mergeSAfallcontigs/mergeSAfSRfall-split.sorted.bam -O bam -T temp mergeSAfallcontigs/mergeSAfSRfall-split.fixmate.bam
/programs/samtools-1.15.1-r/bin/samtools sort -o mergeSAfallcontigs/mergeSAfSSfall-split.sorted.bam -O bam -T temp mergeSAfallcontigs/mergeSAfSSfall-split.fixmate.bam

/programs/samtools-1.15.1-r/bin/samtools index mergeSAfallcontigs/mergeSAfall-split.sorted.bam
/programs/samtools-1.15.1-r/bin/samtools index mergeSAfallcontigs/mergeSAfS3mid-split.sorted.bam
/programs/samtools-1.15.1-r/bin/samtools index mergeSAfallcontigs/mergeSAfSAmid-split.sorted.bam
/programs/samtools-1.15.1-r/bin/samtools index mergeSAfallcontigs/mergeSAfSRmid-split.sorted.bam
/programs/samtools-1.15.1-r/bin/samtools index mergeSAfallcontigs/mergeSAfSSmid-split.sorted.bam
/programs/samtools-1.15.1-r/bin/samtools index mergeSAfallcontigs/mergeSAfS3fall-split.sorted.bam
/programs/samtools-1.15.1-r/bin/samtools index mergeSAfallcontigs/mergeSAfSRfall-split.sorted.bam
/programs/samtools-1.15.1-r/bin/samtools index mergeSAfallcontigs/mergeSAfSSfall-split.sorted.bam


#### SR fall
/programs/bwa-0.7.17/bwa index mergeSRfallcontigs/mergeSRfallcontigs-SPLITS.fa 

/programs/bwa-0.7.17/bwa mem mergeSRfallcontigs/mergeSRfallcontigs-SPLITS.fa /workdir/eag252/cystmetagenome_all/SRcystsfall_R1.fastq.gz /workdir/eag252/cystmetagenome_all/SRcystsfall_R2.fastq.gz > mergeSRfallcontigs/mergeSRfall-split.sam -t 40
/programs/bwa-0.7.17/bwa mem mergeSRfallcontigs/mergeSRfallcontigs-SPLITS.fa /workdir/eag252/cystmetagenome_all/S3cystsmid_R1.fastq.gz /workdir/eag252/cystmetagenome_all/S3cystsmid_R2.fastq.gz > mergeSRfallcontigs/mergeSRfS3mid-split.sam -t 40
/programs/bwa-0.7.17/bwa mem mergeSRfallcontigs/mergeSRfallcontigs-SPLITS.fa /workdir/eag252/cystmetagenome_all/SAcystsmid_R1.fastq.gz /workdir/eag252/cystmetagenome_all/SAcystsmid_R2.fastq.gz > mergeSRfallcontigs/mergeSRfSAmid-split.sam -t 40
/programs/bwa-0.7.17/bwa mem mergeSRfallcontigs/mergeSRfallcontigs-SPLITS.fa /workdir/eag252/cystmetagenome_all/SRcystsmid_R1.fastq.gz /workdir/eag252/cystmetagenome_all/SRcystsmid_R2.fastq.gz > mergeSRfallcontigs/mergeSRfSRmid-split.sam -t 40
/programs/bwa-0.7.17/bwa mem mergeSRfallcontigs/mergeSRfallcontigs-SPLITS.fa /workdir/eag252/cystmetagenome_all/SScystsmid_R1.fastq.gz /workdir/eag252/cystmetagenome_all/SScystsmid_R2.fastq.gz > mergeSRfallcontigs/mergeSRfSSmid-split.sam -t 40
/programs/bwa-0.7.17/bwa mem mergeSRfallcontigs/mergeSRfallcontigs-SPLITS.fa /workdir/eag252/cystmetagenome_all/S3cystsfall_R1.fastq.gz /workdir/eag252/cystmetagenome_all/S3cystsfall_R2.fastq.gz > mergeSRfallcontigs/mergeSRfS3fall-split.sam -t 40
/programs/bwa-0.7.17/bwa mem mergeSRfallcontigs/mergeSRfallcontigs-SPLITS.fa /workdir/eag252/cystmetagenome_all/SAcystsfall_R1.fastq.gz /workdir/eag252/cystmetagenome_all/SAcystsfall_R2.fastq.gz > mergeSRfallcontigs/mergeSRfSAfall-split.sam -t 40
/programs/bwa-0.7.17/bwa mem mergeSRfallcontigs/mergeSRfallcontigs-SPLITS.fa /workdir/eag252/cystmetagenome_all/SScystsfall_R1.fastq.gz /workdir/eag252/cystmetagenome_all/SScystsfall_R2.fastq.gz > mergeSRfallcontigs/mergeSRfSSfall-split.sam -t 40

/programs/samtools-1.15.1-r/bin/samtools fixmate -O bam mergeSRfallcontigs/mergeSRfall-split.sam mergeSRfallcontigs/mergeSRfall-split.fixmate.bam
/programs/samtools-1.15.1-r/bin/samtools fixmate -O bam mergeSRfallcontigs/mergeSRfS3mid-split.sam mergeSRfallcontigs/mergeSRfS3mid-split.fixmate.bam
/programs/samtools-1.15.1-r/bin/samtools fixmate -O bam mergeSRfallcontigs/mergeSRfSAmid-split.sam mergeSRfallcontigs/mergeSRfSAmid-split.fixmate.bam
/programs/samtools-1.15.1-r/bin/samtools fixmate -O bam mergeSRfallcontigs/mergeSRfSRmid-split.sam mergeSRfallcontigs/mergeSRfSRmid-split.fixmate.bam
/programs/samtools-1.15.1-r/bin/samtools fixmate -O bam mergeSRfallcontigs/mergeSRfSSmid-split.sam mergeSRfallcontigs/mergeSRfSSmid-split.fixmate.bam
/programs/samtools-1.15.1-r/bin/samtools fixmate -O bam mergeSRfallcontigs/mergeSRfS3fall-split.sam mergeSRfallcontigs/mergeSRfS3fall-split.fixmate.bam
/programs/samtools-1.15.1-r/bin/samtools fixmate -O bam mergeSRfallcontigs/mergeSRfSAfall-split.sam mergeSRfallcontigs/mergeSRfSAfall-split.fixmate.bam
/programs/samtools-1.15.1-r/bin/samtools fixmate -O bam mergeSRfallcontigs/mergeSRfSSfall-split.sam mergeSRfallcontigs/mergeSRfSSfall-split.fixmate.bam

/programs/samtools-1.15.1-r/bin/samtools sort -o mergeSRfallcontigs/mergeSRfall-split.sorted.bam -O bam -T temp mergeSRfallcontigs/mergeSRfall-split.fixmate.bam
/programs/samtools-1.15.1-r/bin/samtools sort -o mergeSRfallcontigs/mergeSRfS3mid-split.sorted.bam -O bam -T temp mergeSRfallcontigs/mergeSRfS3mid-split.fixmate.bam
/programs/samtools-1.15.1-r/bin/samtools sort -o mergeSRfallcontigs/mergeSRfSAmid-split.sorted.bam -O bam -T temp mergeSRfallcontigs/mergeSRfSAmid-split.fixmate.bam
/programs/samtools-1.15.1-r/bin/samtools sort -o mergeSRfallcontigs/mergeSRfSRmid-split.sorted.bam -O bam -T temp mergeSRfallcontigs/mergeSRfSRmid-split.fixmate.bam
/programs/samtools-1.15.1-r/bin/samtools sort -o mergeSRfallcontigs/mergeSRfSSmid-split.sorted.bam -O bam -T temp mergeSRfallcontigs/mergeSRfSSmid-split.fixmate.bam
/programs/samtools-1.15.1-r/bin/samtools sort -o mergeSRfallcontigs/mergeSRfS3fall-split.sorted.bam -O bam -T temp mergeSRfallcontigs/mergeSRfS3fall-split.fixmate.bam
/programs/samtools-1.15.1-r/bin/samtools sort -o mergeSRfallcontigs/mergeSRfSAfall-split.sorted.bam -O bam -T temp mergeSRfallcontigs/mergeSRfSAfall-split.fixmate.bam
/programs/samtools-1.15.1-r/bin/samtools sort -o mergeSRfallcontigs/mergeSRfSSfall-split.sorted.bam -O bam -T temp mergeSRfallcontigs/mergeSRfSSfall-split.fixmate.bam

/programs/samtools-1.15.1-r/bin/samtools index mergeSRfallcontigs/mergeSRfall-split.sorted.bam
/programs/samtools-1.15.1-r/bin/samtools index mergeSRfallcontigs/mergeSRfS3mid-split.sorted.bam
/programs/samtools-1.15.1-r/bin/samtools index mergeSRfallcontigs/mergeSRfSAmid-split.sorted.bam
/programs/samtools-1.15.1-r/bin/samtools index mergeSRfallcontigs/mergeSRfSRmid-split.sorted.bam
/programs/samtools-1.15.1-r/bin/samtools index mergeSRfallcontigs/mergeSRfSSmid-split.sorted.bam
/programs/samtools-1.15.1-r/bin/samtools index mergeSRfallcontigs/mergeSRfS3fall-split.sorted.bam
/programs/samtools-1.15.1-r/bin/samtools index mergeSRfallcontigs/mergeSRfSAfall-split.sorted.bam
/programs/samtools-1.15.1-r/bin/samtools index mergeSRfallcontigs/mergeSRfSSfall-split.sorted.bam

#### SS fall
/programs/bwa-0.7.17/bwa index mergeSSfallcontigs/mergeSSfallcontigs-SPLITS.fa 

/programs/bwa-0.7.17/bwa mem mergeSSfallcontigs/mergeSSfallcontigs-SPLITS.fa /workdir/eag252/cystmetagenome_all/SScystsfall_R1.fastq.gz /workdir/eag252/cystmetagenome_all/SScystsfall_R2.fastq.gz > mergeSSfallcontigs/mergeSSfall-split.sam -t 40
/programs/bwa-0.7.17/bwa mem mergeSSfallcontigs/mergeSSfallcontigs-SPLITS.fa /workdir/eag252/cystmetagenome_all/S3cystsmid_R1.fastq.gz /workdir/eag252/cystmetagenome_all/S3cystsmid_R2.fastq.gz > mergeSSfallcontigs/mergeSSfS3mid-split.sam -t 40
/programs/bwa-0.7.17/bwa mem mergeSSfallcontigs/mergeSSfallcontigs-SPLITS.fa /workdir/eag252/cystmetagenome_all/SAcystsmid_R1.fastq.gz /workdir/eag252/cystmetagenome_all/SAcystsmid_R2.fastq.gz > mergeSSfallcontigs/mergeSSfSAmid-split.sam -t 40
/programs/bwa-0.7.17/bwa mem mergeSSfallcontigs/mergeSSfallcontigs-SPLITS.fa /workdir/eag252/cystmetagenome_all/SRcystsmid_R1.fastq.gz /workdir/eag252/cystmetagenome_all/SRcystsmid_R2.fastq.gz > mergeSSfallcontigs/mergeSSfSRmid-split.sam -t 40
/programs/bwa-0.7.17/bwa mem mergeSSfallcontigs/mergeSSfallcontigs-SPLITS.fa /workdir/eag252/cystmetagenome_all/SScystsmid_R1.fastq.gz /workdir/eag252/cystmetagenome_all/SScystsmid_R2.fastq.gz > mergeSSfallcontigs/mergeSSfSSmid-split.sam -t 40
/programs/bwa-0.7.17/bwa mem mergeSSfallcontigs/mergeSSfallcontigs-SPLITS.fa /workdir/eag252/cystmetagenome_all/S3cystsfall_R1.fastq.gz /workdir/eag252/cystmetagenome_all/S3cystsfall_R2.fastq.gz > mergeSSfallcontigs/mergeSSfS3fall-split.sam -t 40
/programs/bwa-0.7.17/bwa mem mergeSSfallcontigs/mergeSSfallcontigs-SPLITS.fa /workdir/eag252/cystmetagenome_all/SAcystsfall_R1.fastq.gz /workdir/eag252/cystmetagenome_all/SAcystsfall_R2.fastq.gz > mergeSSfallcontigs/mergeSSfSAfall-split.sam -t 40
/programs/bwa-0.7.17/bwa mem mergeSSfallcontigs/mergeSSfallcontigs-SPLITS.fa /workdir/eag252/cystmetagenome_all/SRcystsfall_R1.fastq.gz /workdir/eag252/cystmetagenome_all/SRcystsfall_R2.fastq.gz > mergeSSfallcontigs/mergeSSfSRfall-split.sam -t 40

/programs/samtools-1.15.1-r/bin/samtools fixmate -O bam mergeSSfallcontigs/mergeSSfall-split.sam mergeSSfallcontigs/mergeSSfall-split.fixmate.bam
/programs/samtools-1.15.1-r/bin/samtools fixmate -O bam mergeSSfallcontigs/mergeSSfS3mid-split.sam mergeSSfallcontigs/mergeSSfS3mid-split.fixmate.bam
/programs/samtools-1.15.1-r/bin/samtools fixmate -O bam mergeSSfallcontigs/mergeSSfSAmid-split.sam mergeSSfallcontigs/mergeSSfSAmid-split.fixmate.bam
/programs/samtools-1.15.1-r/bin/samtools fixmate -O bam mergeSSfallcontigs/mergeSSfSRmid-split.sam mergeSSfallcontigs/mergeSSfSRmid-split.fixmate.bam
/programs/samtools-1.15.1-r/bin/samtools fixmate -O bam mergeSSfallcontigs/mergeSSfSSmid-split.sam mergeSSfallcontigs/mergeSSfSSmid-split.fixmate.bam
/programs/samtools-1.15.1-r/bin/samtools fixmate -O bam mergeSSfallcontigs/mergeSSfS3fall-split.sam mergeSSfallcontigs/mergeSSfS3fall-split.fixmate.bam
/programs/samtools-1.15.1-r/bin/samtools fixmate -O bam mergeSSfallcontigs/mergeSSfSAfall-split.sam mergeSSfallcontigs/mergeSSfSAfall-split.fixmate.bam
/programs/samtools-1.15.1-r/bin/samtools fixmate -O bam mergeSSfallcontigs/mergeSSfSRfall-split.sam mergeSSfallcontigs/mergeSSfSRfall-split.fixmate.bam

/programs/samtools-1.15.1-r/bin/samtools sort -o mergeSSfallcontigs/mergeSSfall-split.sorted.bam -O bam -T temp mergeSSfallcontigs/mergeSSfall-split.fixmate.bam
/programs/samtools-1.15.1-r/bin/samtools sort -o mergeSSfallcontigs/mergeSSfS3mid-split.sorted.bam -O bam -T temp mergeSSfallcontigs/mergeSSfS3mid-split.fixmate.bam
/programs/samtools-1.15.1-r/bin/samtools sort -o mergeSSfallcontigs/mergeSSfSAmid-split.sorted.bam -O bam -T temp mergeSSfallcontigs/mergeSSfSAmid-split.fixmate.bam
/programs/samtools-1.15.1-r/bin/samtools sort -o mergeSSfallcontigs/mergeSSfSRmid-split.sorted.bam -O bam -T temp mergeSSfallcontigs/mergeSSfSRmid-split.fixmate.bam
/programs/samtools-1.15.1-r/bin/samtools sort -o mergeSSfallcontigs/mergeSSfSSmid-split.sorted.bam -O bam -T temp mergeSSfallcontigs/mergeSSfSSmid-split.fixmate.bam
/programs/samtools-1.15.1-r/bin/samtools sort -o mergeSSfallcontigs/mergeSSfS3fall-split.sorted.bam -O bam -T temp mergeSSfallcontigs/mergeSSfS3fall-split.fixmate.bam
/programs/samtools-1.15.1-r/bin/samtools sort -o mergeSSfallcontigs/mergeSSfSAfall-split.sorted.bam -O bam -T temp mergeSSfallcontigs/mergeSSfSAfall-split.fixmate.bam
/programs/samtools-1.15.1-r/bin/samtools sort -o mergeSSfallcontigs/mergeSSfSRfall-split.sorted.bam -O bam -T temp mergeSSfallcontigs/mergeSSfSRfall-split.fixmate.bam

/programs/samtools-1.15.1-r/bin/samtools index mergeSSfallcontigs/mergeSSfall-split.sorted.bam
/programs/samtools-1.15.1-r/bin/samtools index mergeSSfallcontigs/mergeSSfS3mid-split.sorted.bam
/programs/samtools-1.15.1-r/bin/samtools index mergeSSfallcontigs/mergeSSfSAmid-split.sorted.bam
/programs/samtools-1.15.1-r/bin/samtools index mergeSSfallcontigs/mergeSSfSRmid-split.sorted.bam
/programs/samtools-1.15.1-r/bin/samtools index mergeSSfallcontigs/mergeSSfSSmid-split.sorted.bam
/programs/samtools-1.15.1-r/bin/samtools index mergeSSfallcontigs/mergeSSfS3fall-split.sorted.bam
/programs/samtools-1.15.1-r/bin/samtools index mergeSSfallcontigs/mergeSSfSAfall-split.sorted.bam
/programs/samtools-1.15.1-r/bin/samtools index mergeSSfallcontigs/mergeSSfSRfall-split.sorted.bam


### Binning with metabat
runMetaBat.sh -t 40 mergeS3midcontigs/mergeS3midcontigs-SPLITS.fa mergeS3midcontigs/mergeS3mid-split.sorted.bam mergeS3midcontigs/mergeS3mSAmid-split.sorted.bam mergeS3midcontigs/mergeS3mSRmid-split.sorted.bam mergeS3midcontigs/mergeS3mSSmid-split.sorted.bam mergeS3midcontigs/mergeS3mS3fall-split.sorted.bam mergeS3midcontigs/mergeS3mSAfall-split.sorted.bam mergeS3midcontigs/mergeS3mSRfall-split.sorted.bam mergeS3midcontigs/mergeS3mSSfall-split.sorted.bam

runMetaBat.sh -t 40 mergeSAmidcontigs/mergeSAmidcontigs-SPLITS.fa mergeSAmidcontigs/mergeSAmid-split.sorted.bam mergeSAmidcontigs/mergeSAmS3mid-split.sorted.bam mergeSAmidcontigs/mergeSAmSRmid-split.sorted.bam mergeSAmidcontigs/mergeSAmSSmid-split.sorted.bam mergeSAmidcontigs/mergeSAmS3fall-split.sorted.bam mergeSAmidcontigs/mergeSAmSAfall-split.sorted.bam mergeSAmidcontigs/mergeSAmSRfall-split.sorted.bam mergeSAmidcontigs/mergeSAmSSfall-split.sorted.bam

runMetaBat.sh -t 40 mergeSRmidcontigs/mergeSRmidcontigs-SPLITS.fa mergeSRmidcontigs/mergeSRmid-split.sorted.bam mergeSRmidcontigs/mergeSRmS3mid-split.sorted.bam mergeSRmidcontigs/mergeSRmSAmid-split.sorted.bam mergeSRmidcontigs/mergeSRmSSmid-split.sorted.bam mergeSRmidcontigs/mergeSRmS3fall-split.sorted.bam mergeSRmidcontigs/mergeSRmSAfall-split.sorted.bam mergeSRmidcontigs/mergeSRmSRfall-split.sorted.bam mergeSRmidcontigs/mergeSRmSSfall-split.sorted.bam

runMetaBat.sh -t 40 mergeSSmidcontigs/mergeSSmidcontigs-SPLITS.fa  mergeSSmidcontigs/mergeSSmid-split.sorted.bam mergeSSmidcontigs/mergeSSmS3mid-split.sorted.bam mergeSSmidcontigs/mergeSSmSAmid-split.sorted.bam mergeSSmidcontigs/mergeSSmSRmid-split.sorted.bam mergeSSmidcontigs/mergeSSmS3fall-split.sorted.bam mergeSSmidcontigs/mergeSSmSAfall-split.sorted.bam mergeSSmidcontigs/mergeSSmSRfall-split.sorted.bam mergeSSmidcontigs/mergeSSmSSfall-split.sorted.bam

runMetaBat.sh -t 40 mergeS3fallcontigs/mergeS3fallcontigs-SPLITS.fa  mergeS3fallcontigs/mergeS3fall-split.sorted.bam mergeS3fallcontigs/mergeS3fS3mid-split.sorted.bam mergeS3fallcontigs/mergeS3fSAmid-split.sorted.bam mergeS3fallcontigs/mergeS3fSRmid-split.sorted.bam mergeS3fallcontigs/mergeS3fSSmid-split.sorted.bam mergeS3fallcontigs/mergeS3fSAfall-split.sorted.bam mergeS3fallcontigs/mergeS3fSRfall-split.sorted.bam mergeS3fallcontigs/mergeS3fSSfall-split.sorted.bam

runMetaBat.sh -t 40 mergeSAfallcontigs/mergeSAfallcontigs-SPLITS.fa  mergeSAfallcontigs/mergeSAfall-split.sorted.bam mergeSAfallcontigs/mergeSAfS3mid-split.sorted.bam mergeSAfallcontigs/mergeSAfSAmid-split.sorted.bam mergeSAfallcontigs/mergeSAfSRmid-split.sorted.bam mergeSAfallcontigs/mergeSAfSSmid-split.sorted.bam mergeSAfallcontigs/mergeSAfS3fall-split.sorted.bam mergeSAfallcontigs/mergeSAfSRfall-split.sorted.bam mergeSAfallcontigs/mergeSAfSSfall-split.sorted.bam

runMetaBat.sh -t 40 mergeSRfallcontigs/mergeSRfallcontigs-SPLITS.fa  mergeSRfallcontigs/mergeSRfall-split.sorted.bam mergeSRfallcontigs/mergeSRfS3mid-split.sorted.bam mergeSRfallcontigs/mergeSRfSAmid-split.sorted.bam mergeSRfallcontigs/mergeSRfSRmid-split.sorted.bam mergeSRfallcontigs/mergeSRfSSmid-split.sorted.bam mergeSRfallcontigs/mergeSRfS3fall-split.sorted.bam mergeSRfallcontigs/mergeSRfSAfall-split.sorted.bam mergeSRfallcontigs/mergeSRfSSfall-split.sorted.bam

runMetaBat.sh -t 40 mergeSSfallcontigs/mergeSSfallcontigs-SPLITS.fa mergeSSfallcontigs/mergeSSfall-split.sorted.bam mergeSSfallcontigs/mergeSSfS3mid-split.sorted.bam mergeSSfallcontigs/mergeSSfSAmid-split.sorted.bam mergeSSfallcontigs/mergeSSfSRmid-split.sorted.bam mergeSSfallcontigs/mergeSSfSSmid-split.sorted.bam mergeSSfallcontigs/mergeSSfS3fall-split.sorted.bam mergeSSfallcontigs/mergeSSfSAfall-split.sorted.bam mergeSSfallcontigs/mergeSSfSRfall-split.sorted.bam
#Finished 8-17-23



### CheckM2 to get quality information on the bins
source /programs/miniconda3/bin/activate checkm2

checkm2 predict --threads 40 --input mergeS3midcontigs-SPLITS.fa.metabat-bins40 -x .fa --output-directory mergeS3midcontigs-SPLITS.fa.metabat-bins40/checkm2_mergeS3mid --database_path /home/eag252/databases/CheckM2_database/uniref100.KO.1.dmnd

checkm2 predict --threads 40 --input mergeSAmidcontigs-SPLITS.fa.metabat-bins40 -x .fa --output-directory mergeSAmidcontigs-SPLITS.fa.metabat-bins40/checkm2_mergeSAmid --database_path /home/eag252/databases/CheckM2_database/uniref100.KO.1.dmnd

checkm2 predict --threads 40 --input mergeSRmidcontigs-SPLITS.fa.metabat-bins40 -x .fa --output-directory mergeSRmidcontigs-SPLITS.fa.metabat-bins40/checkm2_mergeSRmid --database_path /home/eag252/databases/CheckM2_database/uniref100.KO.1.dmnd

checkm2 predict --threads 40 --input mergeSSmidcontigs-SPLITS.fa.metabat-bins40 -x .fa --output-directory mergeSSmidcontigs-SPLITS.fa.metabat-bins40/checkm2_mergeSSmid --database_path /home/eag252/databases/CheckM2_database/uniref100.KO.1.dmnd

checkm2 predict --threads 40 --input mergeS3fallcontigs-SPLITS.fa.metabat-bins40 -x .fa --output-directory mergeS3fallcontigs-SPLITS.fa.metabat-bins40/checkm2_mergeS3fall --database_path /home/eag252/databases/CheckM2_database/uniref100.KO.1.dmnd

checkm2 predict --threads 40 --input mergeSAfallcontigs-SPLITS.fa.metabat-bins40 -x .fa --output-directory mergeSAfallcontigs-SPLITS.fa.metabat-bins40/checkm2_mergeSAfall --database_path /home/eag252/databases/CheckM2_database/uniref100.KO.1.dmnd

checkm2 predict --threads 40 --input mergeSRfallcontigs-SPLITS.fa.metabat-bins40 -x .fa --output-directory mergeSRfallcontigs-SPLITS.fa.metabat-bins40/checkm2_mergeSRfall --database_path /home/eag252/databases/CheckM2_database/uniref100.KO.1.dmnd

checkm2 predict --threads 40 --input mergeSSfallcontigs-SPLITS.fa.metabat-bins40 -x .fa --output-directory mergeSSfallcontigs-SPLITS.fa.metabat-bins40/checkm2_mergeSSfall --database_path /home/eag252/databases/CheckM2_database/uniref100.KO.1.dmnd


bash Fasta_to_Contigs2Bin.sh -i mergeS3midcontigs-SPLITS.fa.metabat-bins40 -e fa > mergeS3midcontigs-SPLITS_binresults.txt

bash Fasta_to_Contigs2Bin.sh -i mergeSAmidcontigs-SPLITS.fa.metabat-bins40 -e fa > mergeSAmidcontigs-SPLITS_binresults.txt

bash Fasta_to_Contigs2Bin.sh -i mergeSRmidcontigs-SPLITS.fa.metabat-bins40 -e fa > mergeSRmidcontigs-SPLITS_binresults.txt

bash Fasta_to_Contigs2Bin.sh -i mergeSSmidcontigs-SPLITS.fa.metabat-bins40 -e fa > mergeSSmidcontigs-SPLITS_binresults.txt

bash Fasta_to_Contigs2Bin.sh -i mergeS3fallcontigs-SPLITS.fa.metabat-bins40 -e fa > mergeS3fallcontigs-SPLITS_binresults.txt

bash Fasta_to_Contigs2Bin.sh -i mergeSAfallcontigs-SPLITS.fa.metabat-bins40 -e fa > mergeSAfallcontigs-SPLITS_binresults.txt

bash Fasta_to_Contigs2Bin.sh -i mergeSRfallcontigs-SPLITS.fa.metabat-bins40 -e fa > mergeSRfallcontigs-SPLITS_binresults.txt

bash Fasta_to_Contigs2Bin.sh -i mergeSSfallcontigs-SPLITS.fa.metabat-bins40 -e fa > mergeSSfallcontigs-SPLITS_binresults.txt

#### Make sure to change the bin names to have an underscore instead of a period

anvi-import-collection reformatmergeS3midcontigs-SPLITS_binresults.txt -p mergeS3midcontigs/PROFILE.db -c S3midprok.db -C default

anvi-import-collection reformatmergeSAmidcontigs-SPLITS_binresults.txt -p mergeSAmidcontigs/PROFILE.db -c SAmidprok.db -C default

anvi-import-collection reformatmergeSRmidcontigs-SPLITS_binresults.txt -p mergeSRmidcontigs/PROFILE.db -c SRmidprok.db -C default

anvi-import-collection reformatmergeSSmidcontigs-SPLITS_binresults.txt -p mergeSSmidcontigs/PROFILE.db -c SSmidprok.db -C default

anvi-import-collection reformatmergeS3fallcontigs-SPLITS_binresults.txt -p mergeS3fallcontigs/PROFILE.db -c S3fallprok.db -C default

anvi-import-collection reformatmergeSAfallcontigs-SPLITS_binresults.txt -p mergeSAfallcontigs/PROFILE.db -c SAfallprok.db -C default

anvi-import-collection reformatmergeSRfallcontigs-SPLITS_binresults.txt -p mergeSRfallcontigs/PROFILE.db -c SRfallprok.db -C default

anvi-import-collection reformatmergeSSfallcontigs-SPLITS_binresults.txt -p mergeSSfallcontigs/PROFILE.db -c SSfallprok.db -C default


anvi-show-collections-and-bins -p mergeS3midcontigs/PROFILE.db


anvi-summarize -c S3midprok.db -p mergeS3midcontigs/PROFILE.db -C default -o S3mid-summary

anvi-summarize -c SAmidprok.db -p mergeSAmidcontigs/PROFILE.db -C default -o SAmid-summary

anvi-summarize -c SRmidprok.db -p mergeSRmidcontigs/PROFILE.db -C default -o SRmid-summary

anvi-summarize -c SSmidprok.db -p mergeSSmidcontigs/PROFILE.db -C default -o SSmid-summary

anvi-summarize -c S3fallprok.db -p mergeS3fallcontigs/PROFILE.db -C default -o S3fall-summary

anvi-summarize -c SAfallprok.db -p mergeSAfallcontigs/PROFILE.db -C default -o SAfall-summary

anvi-summarize -c SRfallprok.db -p mergeSRfallcontigs/PROFILE.db -C default -o SRfall-summary

anvi-summarize -c SSfallprok.db -p mergeSSfallcontigs/PROFILE.db -C default -o SSfall-summary


### Taxononmy of each of the bins/MAGS with GTDBtk
#### move to the directory /workdir/eag252/gtdb_test/
export OMP_NUM_THREADS=8

export PYTHONPATH=/programs/gtdbtk-2.2.6/lib64/python3.9/site-packages:/programs/gtdbtk-2.2.6/lib/python3.9/site-packages

export PATH=/programs/gtdbtk-2.2.6/bin:/programs/hmmer/binaries:/programs/prodigal-2.6.3:/programs/FastTree-2.1.11:/programs/fastANI-1.32:/programs/pplacer-Linux-v1.1.alpha19:/programs/mash-Linux64-v2.3:$PATH

export GTDBTK_DATA_PATH=/workdir/eag252/gtdb_test/release214

#### make directory for all the bins in /local/workdir/eag252/gtdb_test
mkdir cysts_allbins

cd cysts_allbins

#### make a directory for each specific subset of bins
mkdir S3mid
mkdir SAmid
mkdir SRmid
mkdir SSmid
mkdir S3fall
mkdir SAfall
mkdir SRfall
mkdir SSfall

#### Copy each of the bins into each directory
cp /workdir/eag252/cystmetagenome_all/anvio_all/S3mid-summary/bin_by_bin/bin_*/bin_*-contigs.fa S3mid

cp /workdir/eag252/cystmetagenome_all/anvio_all/SAmid-summary/bin_by_bin/bin_*/bin_*-contigs.fa SAmid

cp /workdir/eag252/cystmetagenome_all/anvio_all/SRmid-summary/bin_by_bin/bin_*/bin_*-contigs.fa SRmid

cp /workdir/eag252/cystmetagenome_all/anvio_all/SSmid-summary/bin_by_bin/bin_*/bin_*-contigs.fa SSmid

cp /workdir/eag252/cystmetagenome_all/anvio_all/S3fall-summary/bin_by_bin/bin_*/bin_*-contigs.fa S3fall

cp /workdir/eag252/cystmetagenome_all/anvio_all/SAfall-summary/bin_by_bin/bin_*/bin_*-contigs.fa SAfall

cp /workdir/eag252/cystmetagenome_all/anvio_all/SRfall-summary/bin_by_bin/bin_*/bin_*-contigs.fa SRfall

cp /workdir/eag252/cystmetagenome_all/anvio_all/SSfall-summary/bin_by_bin/bin_*/bin_*-contigs.fa SSfall

#### Run the identify, align and claddify steps for each directory of bins. 
gtdbtk identify --genome_dir S3mid/ --out_dir S3mid_identoutput -x fa --cpus 40

gtdbtk align --identify_dir S3mid_identoutput --out_dir S3mid_alignoutput --cpus 40

gtdbtk classify --genome_dir S3mid/ -x fa --align_dir S3mid_alignoutput --cpus 40 --out_dir S3mid_classifyoutput --skip_ani_screen


gtdbtk identify --genome_dir SAmid/ --out_dir SAmid_identoutput -x fa --cpus 40

gtdbtk align --identify_dir SAmid_identoutput --out_dir SAmid_alignoutput --cpus 40

gtdbtk classify --genome_dir SAmid/ -x fa --align_dir SAmid_alignoutput --cpus 40 --out_dir SAmid_classifyoutput --skip_ani_screen


gtdbtk identify --genome_dir SRmid/ --out_dir SRmid_identoutput -x fa --cpus 40

gtdbtk align --identify_dir SRmid_identoutput --out_dir SRmid_alignoutput --cpus 40

gtdbtk classify --genome_dir SRmid/ -x fa --align_dir SRmid_alignoutput --cpus 40 --out_dir SRmid_classifyoutput --skip_ani_screen


gtdbtk identify --genome_dir SSmid/ --out_dir SSmid_identoutput -x fa --cpus 40

gtdbtk align --identify_dir SSmid_identoutput --out_dir SSmid_alignoutput --cpus 40

gtdbtk classify --genome_dir SSmid/ -x fa --align_dir SSmid_alignoutput --cpus 40 --out_dir SSmid_classifyoutput --skip_ani_screen


gtdbtk identify --genome_dir S3fall/ --out_dir S3fall_identoutput -x fa --cpus 40

gtdbtk align --identify_dir S3fall_identoutput --out_dir S3fall_alignoutput --cpus 40

gtdbtk classify --genome_dir S3fall/ -x fa --align_dir S3fall_alignoutput --cpus 40 --out_dir S3fall_classifyoutput --skip_ani_screen


gtdbtk identify --genome_dir SAfall/ --out_dir SAfall_identoutput -x fa --cpus 40

gtdbtk align --identify_dir SAfall_identoutput --out_dir SAfall_alignoutput --cpus 40

gtdbtk classify --genome_dir SAfall/ -x fa --align_dir SAfall_alignoutput --cpus 40 --out_dir SAfall_classifyoutput --skip_ani_screen


gtdbtk identify --genome_dir SRfall/ --out_dir SRfall_identoutput -x fa --cpus 40

gtdbtk align --identify_dir SRfall_identoutput --out_dir SRfall_alignoutput --cpus 40

gtdbtk classify --genome_dir SRfall/ -x fa --align_dir SRfall_alignoutput --cpus 40 --out_dir SRfall_classifyoutput --skip_ani_screen


gtdbtk identify --genome_dir SSfall/ --out_dir SSfall_identoutput -x fa --cpus 40

gtdbtk align --identify_dir SSfall_identoutput --out_dir SSfall_alignoutput --cpus 40

gtdbtk classify --genome_dir SSfall/ -x fa --align_dir SSfall_alignoutput --cpus 40 --out_dir SSfall_classifyoutput --skip_ani_screen


### HUMAnN 3.0 https://github.com/biobakery/humann
##### recommended uniref90_diamond 

#### This will run humann 3.0
#### Humann on https://biohpc.cornell.edu/lab/userguide.aspx?a=software&i=855#c
export PYTHONPATH=/programs/metaphlan-4.0.6/lib/python3.9/site-packages:/programs/metaphlan-4.0.6/lib64/python3.9/site-packages

export PATH=/programs/metaphlan-4.0.6/bin:$PATH

export PYTHONPATH=/programs/humann-3.7/lib/python3.9/site-packages:$PYTHONPATH

export PATH=/programs/humann-3.7/bin:/programs/diamond:$PATH

#download chocophlan database
humann_databases --download chocophlan full /workdir/eag252/humann/

#uniref database
humann_databases --download uniref uniref90_ec_filtered_diamond  /workdir/eag252/humann/

cat /workdir/eag252/cystmetagenome_all/S3cystsmid_R1.fastq.gz /workdir/eag252/cystmetagenome_all/S3cystsmid_R2.fastq.gz > S3cystsmid_Both.fastq.gz

humann -i S3cystsmid_Both.fastq.gz --input-format fastq -o S3mid_Test4 --nucleotide-database /workdir/eag252/humann/chocophlan/ --protein-database /workdir/eag252/humann/uniref/ --metaphlan-options="--bowtie2db /workdir/eag252/humann/chocophlan" --threads 30 --output-max-decimals 2
#It worked!

#### Try putting the names to the features instead of uniref codes 
humann_renorm_table --input S3mid_Test4/S3cystsmid_Both_genefamilies.tsv --output S3mid_Test4/S3cystsmid_genefamilies_cpm.tsv --units cpm --update-snames

grep -v "#" S3mid_Test4/S3cystsmid_genefamilies_cpm.tsv | grep -v "|" | cut -f2 | paste -s -d+ - | bc
#1000000.47848974

#### Regrouping genes to other functional categories
humann_regroup_table --input S3mid_Test4/S3cystsmid_genefamilies_cpm.tsv --output S3mid_Test4/S3cystsmid_rxn_cpm.tsv --groups uniref90_rxn
#Original Feature Count: 320179; Grouped 1+ times: 86321 (27.0%); Grouped 2+ times: 30676 (9.6%)

grep -v "#" S3mid_Test4/S3cystsmid_rxn_cpm.tsv | grep -v "|" | cut -f2 | paste -s -d+ - | bc
#1205171.12776131707762415

#### Attaching names to features
humann_rename_table --input S3mid_Test4/S3cystsmid_rxn_cpm.tsv --output S3mid_Test4/S3cystsmid_rxn_cpm_named.tsv --names metacyc-rxn
#Renamed 4404 of 4410 entries (99.86%)


https://forum.biobakery.org/t/humann3-paired-end-reads/862
cat /workdir/eag252/cystmetagenome_all/SAcystsmid_R1.fastq.gz /workdir/eag252/cystmetagenome_all/SAcystsmid_R2.fastq.gz > SAcystsmid_Both.fastq.gz

cat /workdir/eag252/cystmetagenome_all/SRcystsmid_R1.fastq.gz /workdir/eag252/cystmetagenome_all/SRcystsmid_R2.fastq.gz > SRcystsmid_Both.fastq.gz

cat /workdir/eag252/cystmetagenome_all/SScystsmid_R1.fastq.gz /workdir/eag252/cystmetagenome_all/SScystsmid_R2.fastq.gz > SScystsmid_Both.fastq.gz

cat /workdir/eag252/cystmetagenome_all/S3cystsfall_R1.fastq.gz /workdir/eag252/cystmetagenome_all/S3cystsfall_R2.fastq.gz > S3cystsfall_Both.fastq.gz

cat /workdir/eag252/cystmetagenome_all/SAcystsfall_R1.fastq.gz /workdir/eag252/cystmetagenome_all/SAcystsfall_R2.fastq.gz > SAcystsfall_Both.fastq.gz

cat /workdir/eag252/cystmetagenome_all/SRcystsfall_R1.fastq.gz /workdir/eag252/cystmetagenome_all/SRcystsfall_R2.fastq.gz > SRcystsfall_Both.fastq.gz

cat /workdir/eag252/cystmetagenome_all/SScystsfall_R1.fastq.gz /workdir/eag252/cystmetagenome_all/SScystsfall_R2.fastq.gz > SScystsfall_Both.fastq.gz

#### unzip the fastq files for humann


humann -i SAcystsmid_Both.fastq --input-format fastq -o SAmid --nucleotide-database /workdir/eag252/humann/chocophlan/ --protein-database /workdir/eag252/humann/uniref/ --metaphlan-options="--bowtie2db /workdir/eag252/humann/chocophlan" --threads 30 --output-max-decimals 2

humann -i SRcystsmid_Both.fastq --input-format fastq -o SRmid --nucleotide-database /workdir/eag252/humann/chocophlan/ --protein-database /workdir/eag252/humann/uniref/ --metaphlan-options="--bowtie2db /workdir/eag252/humann/chocophlan" --threads 30 --output-max-decimals 2

humann -i SScystsmid_Both.fastq --input-format fastq -o SSmid --nucleotide-database /workdir/eag252/humann/chocophlan/ --protein-database /workdir/eag252/humann/uniref/ --metaphlan-options="--bowtie2db /workdir/eag252/humann/chocophlan" --threads 30 --output-max-decimals 2

humann -i S3cystsfall_Both.fastq --input-format fastq -o S3fall --nucleotide-database /workdir/eag252/humann/chocophlan/ --protein-database /workdir/eag252/humann/uniref/ --metaphlan-options="--bowtie2db /workdir/eag252/humann/chocophlan" --threads 30 --output-max-decimals 2

humann -i SAcystsfall_Both.fastq --input-format fastq -o SAfall --nucleotide-database /workdir/eag252/humann/chocophlan/ --protein-database /workdir/eag252/humann/uniref/ --metaphlan-options="--bowtie2db /workdir/eag252/humann/chocophlan" --threads 30 --output-max-decimals 2

humann -i SRcystsfall_Both.fastq --input-format fastq -o SRfall --nucleotide-database /workdir/eag252/humann/chocophlan/ --protein-database /workdir/eag252/humann/uniref/ --metaphlan-options="--bowtie2db /workdir/eag252/humann/chocophlan" --threads 30 --output-max-decimals 2

humann -i SScystsfall_Both.fastq --input-format fastq -o SSfall --nucleotide-database /workdir/eag252/humann/chocophlan/ --protein-database /workdir/eag252/humann/uniref/ --metaphlan-options="--bowtie2db /workdir/eag252/humann/chocophlan" --threads 30 --output-max-decimals 2


#### Try putting the names to the features instead of uniref codes 
humann_renorm_table --input SAmid/SAcystsmid_Both_genefamilies.tsv --output SAmid/SAcystsmid_genefamilies_cpm.tsv --units cpm --update-snames

grep -v "#" SAmid/SAcystsmid_genefamilies_cpm.tsv | grep -v "|" | cut -f2 | paste -s -d+ - | bc

#### Regrouping genes to other functional categories
humann_regroup_table --input SAmid/SAcystsmid_genefamilies_cpm.tsv --output SAmid/SAcystsmid_rxn_cpm.tsv --groups uniref90_rxn

grep -v "#" SAmid/SAcystsmid_rxn_cpm.tsv | grep -v "|" | cut -f2 | paste -s -d+ - | bc

#### Attaching names to features
humann_rename_table --input SAmid/SAcystsmid_rxn_cpm.tsv --output SAmid/SAcystsmid_rxn_cpm_named.tsv --names metacyc-rxn


humann_renorm_table --input SRmid/SRcystsmid_Both_genefamilies.tsv --output SRmid/SRcystsmid_genefamilies_cpm.tsv --units cpm --update-snames

grep -v "#" SRmid/SRcystsmid_genefamilies_cpm.tsv | grep -v "|" | cut -f2 | paste -s -d+ - | bc

humann_regroup_table --input SRmid/SRcystsmid_genefamilies_cpm.tsv --output SRmid/SRcystsmid_rxn_cpm.tsv --groups uniref90_rxn

grep -v "#" SRmid/SRcystsmid_rxn_cpm.tsv | grep -v "|" | cut -f2 | paste -s -d+ - | bc

humann_rename_table --input SRmid/SRcystsmid_rxn_cpm.tsv --output SRmid/SRcystsmid_rxn_cpm_named.tsv --names metacyc-rxn


humann_renorm_table --input SSmid/SScystsmid_Both_genefamilies.tsv --output SSmid/SScystsmid_genefamilies_cpm.tsv --units cpm --update-snames

grep -v "#" SSmid/SScystsmid_genefamilies_cpm.tsv | grep -v "|" | cut -f2 | paste -s -d+ - | bc

humann_regroup_table --input SSmid/SScystsmid_genefamilies_cpm.tsv --output SSmid/SScystsmid_rxn_cpm.tsv --groups uniref90_rxn

grep -v "#" SSmid/SScystsmid_rxn_cpm.tsv | grep -v "|" | cut -f2 | paste -s -d+ - | bc

humann_rename_table --input SSmid/SScystsmid_rxn_cpm.tsv --output SSmid/SScystsmid_rxn_cpm_named.tsv --names metacyc-rxn


humann_renorm_table --input S3fall/S3cystsfall_Both_genefamilies.tsv --output S3fall/S3cystsfall_genefamilies_cpm.tsv --units cpm --update-snames

grep -v "#" S3fall/S3cystsfall_genefamilies_cpm.tsv | grep -v "|" | cut -f2 | paste -s -d+ - | bc

humann_regroup_table --input S3fall/S3cystsfall_genefamilies_cpm.tsv --output S3fall/S3cystsfall_rxn_cpm.tsv --groups uniref90_rxn

grep -v "#" S3fall/S3cystsfall_rxn_cpm.tsv | grep -v "|" | cut -f2 | paste -s -d+ - | bc

humann_rename_table --input S3fall/S3cystsfall_rxn_cpm.tsv --output S3fall/S3cystsfall_rxn_cpm_named.tsv --names metacyc-rxn


humann_renorm_table --input SAfall/SAcystsfall_Both_genefamilies.tsv --output SAfall/SAcystsfall_genefamilies_cpm.tsv --units cpm --update-snames

grep -v "#" SAfall/SAcystsfall_genefamilies_cpm.tsv | grep -v "|" | cut -f2 | paste -s -d+ - | bc

humann_regroup_table --input SAfall/SAcystsfall_genefamilies_cpm.tsv --output SAfall/SAcystsfall_rxn_cpm.tsv --groups uniref90_rxn

grep -v "#" SAfall/SAcystsfall_rxn_cpm.tsv | grep -v "|" | cut -f2 | paste -s -d+ - | bc

humann_rename_table --input SAfall/SAcystsfall_rxn_cpm.tsv --output SAfall/SAcystsfall_rxn_cpm_named.tsv --names metacyc-rxn


humann_renorm_table --input SRfall/SRcystsfall_Both_genefamilies.tsv --output SRfall/SRcystsfall_genefamilies_cpm.tsv --units cpm --update-snames

grep -v "#" SRfall/SRcystsfall_genefamilies_cpm.tsv | grep -v "|" | cut -f2 | paste -s -d+ - | bc

humann_regroup_table --input SRfall/SRcystsfall_genefamilies_cpm.tsv --output SRfall/SRcystsfall_rxn_cpm.tsv --groups uniref90_rxn

grep -v "#" SRfall/SRcystsfall_rxn_cpm.tsv | grep -v "|" | cut -f2 | paste -s -d+ - | bc

humann_rename_table --input SRfall/SRcystsfall_rxn_cpm.tsv --output SRfall/SRcystsfall_rxn_cpm_named.tsv --names metacyc-rxn


humann_renorm_table --input SSfall/SScystsfall_Both_genefamilies.tsv --output SSfall/SScystsfall_genefamilies_cpm.tsv --units cpm --update-snames

grep -v "#" SSfall/SScystsfall_genefamilies_cpm.tsv | grep -v "|" | cut -f2 | paste -s -d+ - | bc

humann_regroup_table --input SSfall/SScystsfall_genefamilies_cpm.tsv --output SSfall/SScystsfall_rxn_cpm.tsv --groups uniref90_rxn

grep -v "#" SSfall/SScystsfall_rxn_cpm.tsv | grep -v "|" | cut -f2 | paste -s -d+ - | bc

humann_rename_table --input SSfall/SScystsfall_rxn_cpm.tsv --output SSfall/SScystsfall_rxn_cpm_named.tsv --names metacyc-rxn


#### Trying to get output to use for MaAsLin2
https://github.com/biobakery/biobakery/wiki/maaslin2
https://github.com/biobakery/humann#humann_split_stratified_table

humann_split_stratified_table --input S3mid_Test4/S3cystsmid_Both_genefamilies.tsv --output S3mid_Test4
#This removes the non classified taxa counts from the stratified file? The unstratified just has total counts for each gene?
#Can the stratified gene family file, go through the same code as above? adding the real gene names to the uniref codes?



### Binning Eukaryote contigs
#### S3 mid euk
/programs/bwa-0.7.17/bwa index S3_Midassembly/S3_mideuk.fasta

/programs/bwa-0.7.17/bwa mem S3_Midassembly/S3_mideuk.fasta S3cystsmid_R1.fastq.gz S3cystsmid_R2.fastq.gz > S3_Midassembly/S3_mideuk.aligned.sam -t 40

/programs/samtools-1.15.1-r/bin/samtools fixmate -O bam S3_Midassembly/S3_mideuk.aligned.sam S3_Midassembly/S3_mideuk.aligned_fixmate.bam

/programs/samtools-1.15.1-r/bin/samtools sort -o S3_Midassembly/S3_mideuk.aligned_sorted.bam -O bam -T temp S3_Midassembly/S3_mideuk.aligned_fixmate.bam

/programs/samtools-1.15.1-r/bin/samtools index S3_Midassembly/S3_mideuk.aligned_sorted.bam

runMetaBat.sh -t 40 S3_Midassembly/S3_mideuk.fasta S3_Midassembly/S3_mideuk.aligned_sorted.bam

#### SA mid euk
/programs/bwa-0.7.17/bwa index SA_Midassembly/SA_mideuk.fasta

/programs/bwa-0.7.17/bwa mem SA_Midassembly/SA_mideuk.fasta SAcystsmid_R1.fastq.gz SAcystsmid_R2.fastq.gz > SA_Midassembly/SA_mideuk.aligned.sam -t 40

/programs/samtools-1.15.1-r/bin/samtools fixmate -O bam SA_Midassembly/SA_mideuk.aligned.sam SA_Midassembly/SA_mideuk.aligned_fixmate.bam

/programs/samtools-1.15.1-r/bin/samtools sort -o SA_Midassembly/SA_mideuk.aligned_sorted.bam -O bam -T temp SA_Midassembly/SA_mideuk.aligned_fixmate.bam

/programs/samtools-1.15.1-r/bin/samtools index SA_Midassembly/SA_mideuk.aligned_sorted.bam

runMetaBat.sh -t 40 SA_Midassembly/SA_mideuk.fasta SA_Midassembly/SA_mideuk.aligned_sorted.bam

#### SR mid euk
/programs/bwa-0.7.17/bwa index SR_Midassembly/SR_mideuk.fasta

/programs/bwa-0.7.17/bwa mem SR_Midassembly/SR_mideuk.fasta SRcystsmid_R1.fastq.gz SRcystsmid_R2.fastq.gz > SR_Midassembly/SR_mideuk.aligned.sam -t 40

/programs/samtools-1.15.1-r/bin/samtools fixmate -O bam SR_Midassembly/SR_mideuk.aligned.sam SR_Midassembly/SR_mideuk.aligned_fixmate.bam

/programs/samtools-1.15.1-r/bin/samtools sort -o SR_Midassembly/SR_mideuk.aligned_sorted.bam -O bam -T temp SR_Midassembly/SR_mideuk.aligned_fixmate.bam

/programs/samtools-1.15.1-r/bin/samtools index SR_Midassembly/SR_mideuk.aligned_sorted.bam

runMetaBat.sh -t 40 SR_Midassembly/SR_mideuk.fasta SR_Midassembly/SR_mideuk.aligned_sorted.bam

#### SS mid euk
/programs/bwa-0.7.17/bwa index SS_Midassembly/SS_mideuk.fasta

/programs/bwa-0.7.17/bwa mem SS_Midassembly/SS_mideuk.fasta SScystsmid_R1.fastq.gz SScystsmid_R2.fastq.gz > SS_Midassembly/SS_mideuk.aligned.sam -t 40

/programs/samtools-1.15.1-r/bin/samtools fixmate -O bam SS_Midassembly/SS_mideuk.aligned.sam SS_Midassembly/SS_mideuk.aligned_fixmate.bam

/programs/samtools-1.15.1-r/bin/samtools sort -o SS_Midassembly/SS_mideuk.aligned_sorted.bam -O bam -T temp SS_Midassembly/SS_mideuk.aligned_fixmate.bam

/programs/samtools-1.15.1-r/bin/samtools index SS_Midassembly/SS_mideuk.aligned_sorted.bam

runMetaBat.sh -t 40 SS_Midassembly/SS_mideuk.fasta SS_Midassembly/SS_mideuk.aligned_sorted.bam

#### S3 fall euk
/programs/bwa-0.7.17/bwa index S3_Fallassembly/S3_falleuk.fasta

/programs/bwa-0.7.17/bwa mem S3_Fallassembly/S3_falleuk.fasta S3cystsfall_R1.fastq.gz S3cystsfall_R2.fastq.gz > S3_Fallassembly/S3_falleuk.aligned.sam -t 40

/programs/samtools-1.15.1-r/bin/samtools fixmate -O bam S3_Fallassembly/S3_falleuk.aligned.sam S3_Fallassembly/S3_falleuk.aligned_fixmate.bam

/programs/samtools-1.15.1-r/bin/samtools sort -o S3_Fallassembly/S3_falleuk.aligned_sorted.bam -O bam -T temp S3_Fallassembly/S3_falleuk.aligned_fixmate.bam

/programs/samtools-1.15.1-r/bin/samtools index S3_Fallassembly/S3_falleuk.aligned_sorted.bam

runMetaBat.sh -t 40 S3_Fallassembly/S3_falleuk.fasta S3_Fallassembly/S3_falleuk.aligned_sorted.bam

#### SA fall euk
/programs/bwa-0.7.17/bwa index SA_Fallassembly/SA_falleuk.fasta

/programs/bwa-0.7.17/bwa mem SA_Fallassembly/SA_falleuk.fasta SAcystsfall_R1.fastq.gz SAcystsfall_R2.fastq.gz > SA_Fallassembly/SA_falleuk.aligned.sam -t 40

/programs/samtools-1.15.1-r/bin/samtools fixmate -O bam SA_Fallassembly/SA_falleuk.aligned.sam SA_Fallassembly/SA_falleuk.aligned_fixmate.bam

/programs/samtools-1.15.1-r/bin/samtools sort -o SA_Fallassembly/SA_falleuk.aligned_sorted.bam -O bam -T temp SA_Fallassembly/SA_falleuk.aligned_fixmate.bam

/programs/samtools-1.15.1-r/bin/samtools index SA_Fallassembly/SA_falleuk.aligned_sorted.bam

runMetaBat.sh -t 40 SA_Fallassembly/SA_falleuk.fasta SA_Fallassembly/SA_falleuk.aligned_sorted.bam

#### SR fall euk
/programs/bwa-0.7.17/bwa index SR_Fallassembly/SR_falleuk.fasta

/programs/bwa-0.7.17/bwa mem SR_Fallassembly/SR_falleuk.fasta SRcystsfall_R1.fastq.gz SRcystsfall_R2.fastq.gz > SR_Fallassembly/SR_falleuk.aligned.sam -t 40

/programs/samtools-1.15.1-r/bin/samtools fixmate -O bam SR_Fallassembly/SR_falleuk.aligned.sam SR_Fallassembly/SR_falleuk.aligned_fixmate.bam

/programs/samtools-1.15.1-r/bin/samtools sort -o SR_Fallassembly/SR_falleuk.aligned_sorted.bam -O bam -T temp SR_Fallassembly/SR_falleuk.aligned_fixmate.bam

/programs/samtools-1.15.1-r/bin/samtools index SR_Fallassembly/SR_falleuk.aligned_sorted.bam

runMetaBat.sh -t 40 SR_Fallassembly/SR_falleuk.fasta SR_Fallassembly/SR_falleuk.aligned_sorted.bam

#### SS fall euk
/programs/bwa-0.7.17/bwa index SS_Fallassembly/SS_falleuk.fasta

/programs/bwa-0.7.17/bwa mem SS_Fallassembly/SS_falleuk.fasta SScystsfall_R1.fastq.gz SScystsfall_R2.fastq.gz > SS_Fallassembly/SS_falleuk.aligned.sam -t 40

/programs/samtools-1.15.1-r/bin/samtools fixmate -O bam SS_Fallassembly/SS_falleuk.aligned.sam SS_Fallassembly/SS_falleuk.aligned_fixmate.bam

/programs/samtools-1.15.1-r/bin/samtools sort -o SS_Fallassembly/SS_falleuk.aligned_sorted.bam -O bam -T temp SS_Fallassembly/SS_falleuk.aligned_fixmate.bam

/programs/samtools-1.15.1-r/bin/samtools index SS_Fallassembly/SS_falleuk.aligned_sorted.bam

runMetaBat.sh -t 40 SS_Fallassembly/SS_falleuk.fasta SS_Fallassembly/SS_falleuk.aligned_sorted.bam
