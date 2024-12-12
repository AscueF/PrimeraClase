```r
## Como instalar paquetes
## sudo apt install _____

#install.packages('BiocManager')
library(BiocManager)
#BiocManager::install('dada2')
library(dada2)

#### Datos de Pre-procesamiento

lecturas$name[5]

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

## Vemos el nombre filas
rownames(seqtab_nochim)

## Columnas 

colnames(seqtab_nochim)

## Asignacion taxonomica 

ruta <- "data/silva_nr99_v138_train_set.fa.gz"

taxa <- assignTaxonomy(seqtab_nochim, ruta)

### Vamos a trabajar con objetos
### Phyloseq 

#Otra forma
install.packages("devtools")
library(devtools)
install_github('joey711/phyloseq')

BiocManager::install(version = "3.20")

library(phyloseq) #Activar el paquete

metadata <- read.csv('data/metadatos.csv', row.names = 1)
#rownames(metadata) <- metadata$id

phyloseq_obj <- phyloseq( otu_table(seqtab_nochim, taxa_are_rows = F) , 
                          sample_data(metadata) , 
                          tax_table(taxa) )


dna <- Biostrings::DNAStringSet(taxa_names(phyloseq_obj))
names(dna) <- taxa_names(phyloseq_obj)
ps_ob <- merge_phyloseq(phyloseq_obj, dna)

taxa_names(ps_ob) <- paste0('ASV',seq(ntaxa(ps_ob)))
names(dna) <- taxa_names(ps_ob)

Biostrings::writeXStringSet(dna, 'resultados/asv_seq.fasta')
tax_table(ps_ob)

saveRDS(ps_ob, "resultados/ps_ob.RDS")


###### Rarefacción de datos

ps_ob_rar <- rarefy_even_depth(ps_ob, sample.size = 1200 )

saveRDS(ps_ob_rar, 'resultados/ps_ob_rar.RDS')

###### Plots de diversidad 
BiocManager::install("microbiome")
BiocManager::install("ComplexHeatmap")

install.packages(
  "microViz",
  repos = c(davidbarnett = "https://david-barnett.r-universe.dev", getOption("repos"))
)

library(microViz)
library(ggplot2)
install.packages('ggthemes')
library(ggthemes)

ps_rar <- ps_ob_rar %>%  tax_fix()
ps_rar <- ps_rar %>%  tax_fix(unknowns = c('Unknown Family','endosymbionts'))

ps_rar %>% comp_barplot('Genus', n_taxa = 5, merge_other = TRUE) +
  facet_wrap(vars(location), scales ='free')+
  coord_flip()+
  ggtitle('Location ComportPlot')+
  theme_economist()
```

