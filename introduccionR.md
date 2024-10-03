## Practica 4
 
```r
### Para ubicar la carpeta de trabajo podemos cambiar la 
setwd("IntroduccionR/")
getwd() ### Verificamos donde estamos 

dir.create('Resultados') ## Crear directorio

#### Operaciones estadisticas Basicas

9%%5 ## Modulo 

#### Crear Variables

x = 5 
x <- 12 ## Por defecto R recomienda <-( asignacion )
### Si ejecutamos variables con el mismo nombre se sobreescribe 

### Estructuras de datos en R

#### Tipos de datos 

5 # numero 
'a' # caracter
FALSE # Booleano 

#### Vectores y Listas

vec1 <- c(1,2,32,4,5,6) # vector - Guarda un solo tipo de datos
lis1 <- list(1,2,32,'a',4,5,6) # lista - Admite varios tipos de datos

  #### Como accedo a un dato individual 
vec1[6]

#### Matriz y DataFrame (Tabla)

mat <- matrix(vec1,ncol = 2, nrow = 3)

mat2 <- matrix(1:100, ncol = 10, nrow = 10 ) ## 1:100 para 100 datos

#### Como accedo a un dato individual 

mat[2,2] ## Aqui funciona como un plano carteseano 
          ## donde el primer numero antes de la coma
          ## Nos saca la fila y el segundo la columna


## preparamos los datos 
vecA <- c(1,2,3,4)
vecB <- c("A","B","C","D")
vecC <- c(F,T,F,T)

## Creamos el data frame
df <- data.frame(Columna1 = vecA, Columna2 = vecC, Columna3 = vecB )

df[2,3] ## tambien funciona para el Data Frame

df$Columna1 ## Para extraer una columna podemos usar el $
df$Columna1[4] # Cuando extraemos una columna no sale como
              # un vector y tambien podemos aplicar lo anterior
              # vec1[2]


### Funciones en R ( Son operaciones programas que vamos a reutilizar)

sum(1,2) ## Suma 
mean(vec1) ## media
median(vec1) ## mediana
var(vec1) ## Varianza
sd(vec1)

### Como obtengo los datasets por defecto de R
library(help = "datasets")

mean(mtcars$mpg)
matCor <- cor(mtcars)

plot(mtcars$mpg,mtcars$cyl)
barplot(mtcars$mpg)
pie(mtcars$vs)
heatmap(matCor)

## Descomprimir 
unzip('iris.zip')

### Cargar datos a R

doc <- read.table('iris.data', sep = ',' , header = FALSE)
```

