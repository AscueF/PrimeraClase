# Introducción a WSL

Windows Subsystem Linux (Subsistema de Linux para Windows)

**Kernel** -> Windows -> Se monta el SO de Linux `(Ubuntu 22.04)

### Donde vamos a trabajar:

#### "Terminal"

![](https://static1.howtogeekimages.com/wordpress/wp-content/uploads/2023/12/feature.png)

#### Partes de una terminal
```bash
usuario@Lenovo:~$ <codigo>
```
`~` : Este simbolo es el que cambia | ¿por que?:  Señala la ubicacion de nuestra carpeta de trabajo. Al inicio vamos a tener `~` que equivale a una dirección fija: `/home/usuario/` y despues cuando decidamos cambiarnos de ubicación se va a señalar la nuevo dirección.

Para entender esto veamos como se ve una ubicacion en windows:
`C:\Users\usuario\Documentos\Curso Bioinformatica`
 
 En linux el primer  `/` hace referencia a la raiz, que es un caso unico.
 
 ![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhdvPxzJE6ra0IvTxKD0Kw2DDDZTHORNYqQT5A39K0pihlE3HJQannRM7QSmXx79KhuNYrzngGqTUCIGYJ-CLF8wyYOIFNYoDmPSuXzhiBfKW62ve-hDEJJ5OYF3RiyPWoCbXJ4SYpMC2VC/s1600/Linux+File+System.png)
 
#### Comando basicos

**Mostrar el contenido :**
```bash
ls
ls -l
ls -lh ### mostrar en formato humano los pesos
```

**Navegacion en Linux :**
Podemos ayudarnos de la tecla tab para hacer un autocompletado rapido.
> La ayuda del tab solo esta presente si existe la carpeta, para ello debemos darle las primeras letras del nombre.

##### Navegacion hacia Adelante
*forma basica*
```bash
cd Nombre_carpeta
```
*forma intermedia*
```bash
cd Nombre_carpeta/subcarpeta
```

*forma con direcciones globales*
```bash
cd /home/fascue/Documentos
```

*Atajos*
```bash
cd ~/Documentos
```
##### Navegacion hacia Atras

```bash
cd ..
```
*forma intermedia*
```bash
cd ../..
```
