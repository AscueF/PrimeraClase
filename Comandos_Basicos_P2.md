## Practica 2

### Comandos Basicos 2

```bash
ls -l
ls -lh
```

```bash 
cd carpeta
cd /home/usuario/Documentos/
```

#### Caso particular de wsl 
```bash
cd /mnt/c/Users/user/Desktop
```

##### Variables ambientales
Estas variables guardan informacion de mi usuario  (`fascue` : `$USER` ) y del directorio de trabajo que tengo asignado.
```bash
cd $HOME 
cd /home/$USER 
```
El proposito de las variables ambientales, es que podemos **generalizar ejecuciones para cualquier usuario**.

#### Atajos utiles para movilizar
Permite regresar a la ultima ubicacion

```bash
cd ~- 
```

**Guardar una ubicacion**

```bash
UBI=/home/fascue/Documentos/UNSAAC
cd $UBI
```

#### Comados para crear y borrar documentos

**Crear Documentos**

```bash
touch ejemplo.txt
echo 'Algun texto' > ejemplo.txt #reescribe el contenido
echo 'Algun texto' >> ejemplo.txt #no reescribe el contenido
``` 
> Recordatorio
El simbolo `>` reescribe el documento y el simbolo `>>` puede sobreescribir el documento previo.

**Borrar Archivos**

```bash
rm nombre_archivo
```
**Crear Carpetas**

```bash
mkdir Ejemplo ## Vacia
rmdir Ejemplo ## Borra una carpeta vacia
```
>Tips: **Crear Carpeta y subcarpeas a la vez**
>`mkdir -p Carpeta/{subcarpeta1,subcarpeta2}`

> Warning! 
> Para borrar carpetas con contenido usaremos el comando `rm` de la siguiente forma : `rm -r Nombre_carpeta`

**Mover Carpetas o Archivos**

```bash
mv nombre_carpeta carpeta_destino
mv archivo Carpeta_destino
```
**Renombrar Carpetas o Archivos**
```bash
mv nombre_carpeta nuevo_nombre
mv archivo nuevo_nombre
```

### Replicar estos pasos

1. Crear 2 carpetas Ejemplo1 y Ejemplo2 
2. Crear un archivo dentro de Ejemplo1 
3. Ingresar a Ejemplo2 y Traer el Archivo aqui.











