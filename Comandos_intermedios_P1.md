## Practica 3

### Comandos de edicion y AWK
Vamos a crear un archivo o descargar un archivo de internet.
El dataSet [Iris](https://archive.ics.uci.edu/dataset/53/iris) 

Para descomprimir el archivo usamos `unzip iris.zip`
Si no les funciona tienen que instarlo : `sudo apt install zip unzip`

#### Inspeccionar el archivo
Mostramos el **head** (encabezado) o **tail** (cola) de 10 lineas por defecto con el siguiente comando: 
- `head iris.data`
- `tail iris.data`

Para ver todo el archivo: 
- `cat iris.data`
Para evitar saturar la terminal pueden usar : 
- `less iris.data`

#### Editar el archivo   `grep - fgrep` `cut` `sort` `sed` 

#### GREP  o FGREP (alias)

Para usar grep debemos buscar un patron dentro de un archivo o adicionalmente se puede usar en un `pipe`

```bash
grep 'patron de busqueda' archivo.ext
```
En la forma de pipe se veria algo asi:

```bash
cat iris.csv | grep 'patron de busqueda'
```
```bash
 grep 'patron de busqueda' archivo.ext | grep 'busco otra cosa'
```
Para una busquea que incluya tanto minusculas como mayusculas ponemos la opcion `-i` 
```bash
grep -i 'patron de busqueda' archivo.txt
```

*Ejemplo: Filtrar las filas de iris-versicolor y que en los numeros de la fila que haya almenos un 5*

Respuesta: 
```bash
grep "Iris-versicolor" iris.data | grep "5"
```
*Pregunta: ¿Cómo puedo serparar en otro archivo Iris-versicolor unicamente?*

```bash
grep "Iris-versicolor" iris.data > iris_versicolor.data  
```

*Pregunta: Si tengo un encabezado ¿Cómo puedo conservalo en un filtrado?*
Para mostrar solo una fila en head ponemos la opcion `-n 1`
```bash
head -n 1 iris.tsv > iris_versicolor.data
grep "Iris-versicolor" iris.data >> iris_versicolor.data  
```
#### CUT 

Es util para separar columnas, para ello debemos indicarle el separador de columna, por defecto usa el delimitador espacio : `' '`
En ejemplo de iris.data el delimitador es una coma : `','`

```bash
cut -d',' -f 5 iris.csv
# -d : delimitador
# -f : field (campo o columna) 
```
*Pregunta:  ¿Puedo filtrar mas de una columna a la vez?*
Ponemos separado con comas las columnas.
```bash
cut -d',' -f 4,5 iris.csv
```
Si queremos separa varias columnas consecutivas podemos poner el primero y el ultimo separado por un guion : `-`

```bash
cut -d',' -f 1-3 iris.csv
```

#### SORT

Sirve para ordenar los datos de forma numerica y alfabetica, para ello toma los datos con separador  espacio por defecto: `' '`, si no conocen el programa pueden intentar sacar el manual con la opcion: `'-h'` o `'--help'`
 
 Para nuestro caso debemos indicarle que el separador es una coma : `,`

```bash
sort -t',' -k4,4 iris.data
# -d : orden alfabetico (dictionary)
# -n : numerico 
# -t : delimitador
# -k : columna 
# -r : reverso 
sort -t',' -nk4,4r iris.csv
```
Podemos poner la opcion que queramos si ya esta puesto el guion primero : `'-k4,4r'`

*Ejemplo: Inspeccionen el documento iris cual es el valor mas alto de la columna 3 y muestren solamente el head*


#### AWK


#### Comandos extras , utiles para inspeccionar un documento
 
