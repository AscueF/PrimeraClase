```r
## Como instalar paquetes
## sudo apt install _____

install.packages('BiocManager')
library(BiocManager)
BiocManager::install('dada2')
library(dada2)

#### Datos de Pre-procesamiento

### Calidad 

source('scripts/calidad.R')
lecturas <- list_fastq(pattern = c("_1.fastq.gz","_2.fastq.gz"))
plotQualityProfile(c(lecturas$lf[1],lecturas$lr[1]))
  
## Ejercicio : (Plot de 2 pares a la vez)

### Filtrado (trimmed)

source('scripts/filter.R')
log_filter <- filter_reads(name = lecturas$name, lf = lecturas$lf, 
                           lr = lecturas$lr, trunc = 300)

#### METABARCODING 

filtF <- file.path("data/processed_data/filtered_F", paste0(lecturas$name, "_filt_1.fastq"))
filtR <- file.path("data/processed_data/filtered_R", paste0(lecturas$name, "_filt_2.fastq"))

### Modelado de Errores de secuenciamiento 
errR <-learnErrors(filtR, multithread = T)
errF <-learnErrors(filtF, multithread = T)

#grafica del modelo de error
plotErrors(errF,nominalQ = T)
plotErrors(errR,nominalQ = T)


### INFERENCIA DE ASV
dadaF <- dada(filtF, err = errF, multithread = T)
dadaR <- dada(filtR, err = errR, multithread = T)

## merge o pariar o fusion de secuencias----
pareadas <- mergePairs(dadaF, filtF, dadaR, filtR, verbose = T)

seqTab <- makeSequenceTable(pareadas)
seqtab_nochim <- removeBimeraDenovo(seqTab, method = "consensus"
                                    , multithread = T, 
                                    verbose = T)


rownames(seqtab_nochim) <- sub("_filt_1.fastq", "", rownames(seqtab_nochim), fixed = TRUE)

write.csv2(seqtab_nochim,paste0(getwd(),"/resultados/seqtab_nochim.csv"))

### Como guardar objetos en R y volver a cargarlos

saveRDS(seqtab_nochim, "resultados/seqtab_nochim.RDS")

seqtab_nochim <- readRDS('resultados/seqtab_nochim.RDS')


```
