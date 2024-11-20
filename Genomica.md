## Practica 5
 
### Ensamblaje de Genomas 
Para esta practica necesitamos secuencias FASTQ. 
Por ejemplo : 
```bash
-rw-rw-r-- 1 fascue fascue 28875944 oct 9 14:12 SRR30889329_1.fastq.gz  
-rw-rw-r-- 1 fascue fascue 29250967 oct 9 14:12 SRR30889329_2.fastq.gz
```
#### Crear directorio de trabajo 

```bash
mkdir -p Ensamblaje/{data,resultados,scripts}
```
Mover los archivos fastq a la carpeta data, despues creamos una carpeta de calidad en resultados:

```bash
mkdir resultados/calidad
``` 

Luego instalamos fastqc 
```bash
sudo apt install fastqc
```

Y ejecutamos FASTQC

```bash
fastqc -o resultados/calidad *fastq.gz 
```
Es conveniente en algunos casos, tener la calidad conjunta en un solo archivo para ello usaremos el programa `multiqc`

```bash
cd /resultados/calidad
multiqc . # no olvidar indicar el '.'
``` 
### Filtrado de secuencias 
Cuando tienen secuencias `paired end`
```bash
TrimmomaticPE -phred33 
data/SRR30889329_1.fastq.gz 
data/SRR30889329_2.fastq.gz 
resultados/filtrado/f_f-p.fq.gz 
resultados/filtrado/file_f_u.fq.gz 
resultados/filtrado/file_r_p.fq.gz 
resultados/filtrado/file_r_u.fq.gz 
ILLUMINACLIP:TruSeq3-PE.fa:2:30:10 
LEADING:3 TRAILING:3 
SLIDINGWINDOW:4:15: MINLEN:50
```
Cuando tienen secuencias `Single end`

```bash
TrimmomaticSE -phred33 
data/SRR30889329_1.fastq.gz 
resultados/filtrado/file_se.fq.gz 
ILLUMINACLIP:TruSeq3-SE.fa:2:30:10 
LEADING:3 TRAILING:3
SLIDINGWINDOW:4:15 MINLEN:50
```

Resultados : 

```
Input Read Pairs: 258680 Both Surviving: 250306 (96.76%) 
|Forward Only Surviving: 1899 (0.73%) Reverse Only Surviving:  
361 (0.14%)| Dropped: 6114 (2.36%)  
TrimmomaticPE: Completed successfully
```

## Practica 6 

### index del genoma  

  ```bash
mkdir -p data/index  
efetch -db nucleotide -id NC_045512.2 -format fasta > data/index/reference_sars_cov2.fasta
bowtie2-build data/index/*.fasta data/index/sars
```


### Alineamiento del genoma  

  ```bash
mkdir -p resultados/ensamblaje  
bowtie2 --end-to-end -p 4 -x data/index/sars -1 resultados/filtrado/f_f-p.fq.gz -2 resultados/filtra  
do/file_r_p.fq.gz -S resultados/ensamblaje/sars-cov2.sam
```

### Procesar Sam


  ```bash
cd resultados/ensamblaje
samtools view -b --threads 2 sars-cov2.sam | samtools sort -@ 4 -o sars-cov2.sorted.bam -  
  
samtools index sars-cov2.sorted.bam
```


### consenso de genoma

  ```bash
samtools mpileup -A -d 0 -Q 0 sars-cov2.sorted.bam | ivar consensus -p consenso -q 25 -t 0.06 -n N -m 10
```

