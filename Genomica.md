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
