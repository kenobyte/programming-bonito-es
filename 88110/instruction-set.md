**[INCOMPLETO]**
**[PENDIENTE DE REVISIÓN]**

# Juego de Instrucciones

Se van a detallar todas las instrucciones disponibles del lenguaje ensamblador del procesador mc88110. Se dividen en las siguientes categorías. Adicionalmente, se van a incluir algunos ejemplos de uso reales para entender la trascendencia de algunas instrucciones y opciones.

El objetivo de este tema es entender la sintaxis y las peculiaridades de las instrucciones. No se pretende que se use como referencia ni se trata de un apéndice.

## Instrucciones de carga/almacenamiento

Admiten modo de direccionamiento relativo a registro base. Son las instrucciones `ld` y  `st`.

* `ld` carga un contenido de una dirección de memoria en un registro
* `st` almacena el contenido de un registro en una dirección de memoria

La sintaxis de ambas es la siguiente:

```
ld   destino, dir.origen,  desplazamiento;
st   origen,  dir.destino, desplazamiento;
```

Dicho en un esquema:

```
ld     reg <- M(dir+desp)
st     reg -> M(dir+desp)
```

Los argumentos según el esquema son `reg`, `dir` y `desp` en ese orden.

El primer argumento (`reg`) siempre es un **registro dato**.

* En `ld` es el registro que contendrá el dato.
* En `st` es el registro que contiene el dato.

El segundo argumento (`dir`) es un **registro base**. Es un registro que contiene una dirección de memoria.

El tercero `desp` es el **desplazamiento**. Contiene un desplazamiento relativo al registro base. Puede ser otro registro o un número con signo de 16 bits.

Ejemplos:

* Almacenar el valor que contiene `r02` en la dirección `r03 + 15`

  ```
  st r02,r03,15;
  ```

* Almacenar el valor que contiene `r02` en la dirección  `r04`

  ```
  st r02,r04,0;
  ```

* Cargar el contenido de la dirección `r03+r04` en el registro `r02`

  ```
  ld r02,r03,r04;
  ```

Ambas instrucciones admiten varias opciones para cargar partes del registro o leer partes de la memoria:

* `.b` Se transfiere un solo byte.
* `.h` Se transfiere media palabra (la menos significativa).
* `.d` Se tranfiere doble palabra. *No se va a utilizar*

En el siguiente esquema se especifica qué parte del registro corresponde el byte y la media palabra.

```   
r02 =  |0100 1010 1110 1001  0000 1010 1010 0001|
                                      |1010 0001| .b
                            |0000 1010 1010 0001| .h
```

La instrucción `ld` con las opciones `.h` y `.b` realiza una extensión de signo. 

###### Ejemplo

```
org  0x1000
data 0x4AE98AA9
org  0x2000
data 0x4AE90A21

main:
  or  r02,r00,0x1000; Esta instrucción pone en "r02" un "0x1000"
  or  r03,r00,0x2000; Esta instrucción pone en "r03" un "0x2000"

  ld.b  r04,r02,0;
  ld.b  r05,r03,0;

  ld.h  r06,r02,0;
  ld.h  r07,r03,0;

  stop;
```

Supongamos que en las direcciones de memoria 0x1000 y 0x2000 tenemos lo siguiente:

```
0x1000 = |0100 1010 1110 1001  1000 1010 1010 1001| (0x4AE98AA9)
0x2000 = |0100 1010 1110 1001  0000 1010 0010 0001| (0x4AE90A21)
```

Y que los registros `r02` y `r03` valen `0x1000` y `0x2000` respectivamente.

Ahora, llamamos a `ld` con las diferentes opciones.

`ld.b r04,r02,0`. Almacena el contenido de la dirección 0x1000+0 en el registro `r04` y realiza extensión de signo.

```
0x1000 |---- ---- ---- ----  ---- ---- 1010 1001| .b
 r04 = |                               1010 1001|
 r04 = |1111 1111 1111 1111  1111 1111 1010 1001| 0xFFFFFFA9
```

`ld.b r05,r03,0`. Almacena el contenido de la dirección 0x2000+0 en el registro `r05` y realiza extensión de signo.

```
0x2000 |---- ---- ---- ----  ---- ---- 0010 0001| .b
 r05 = |                               0010 0001|
 r05 = |0000 0000 0000 0000  0000 0000 0010 0001| 0x00000021
```

Lo mismo pero con media palabra son las dos instrucciones siguientes

```
0x1000 |---- ---- ---- ----  1000 1010 1010 1001| .h
 r06 = |                     1000 1010 1010 1001|
 r06 = |1111 1111 1111 1111  1111 1111 1010 1001| 0xFFFF8AA9
```

y

```
0x2000 |---- ---- ---- ----  0000 1010 0010 0001| .h
 r07 = |                     0000 1010 0010 0001|
 r07 = |0000 0000 0000 0000  0000 1111 1010 1001| 0x00000A21
```

### Sin extensión de signo

Si no se quiere hacer extensión de signo, se utilizan las siguientes opciones:

* `.bu` Carga un byte sin extender signo
* `.hu` Carga media palabra (la menos significativa) sin extender signo.

Ampliamos el ejemplo anterior

```
org  0x1000
data 0x4AE98AA9
org  0x2000
data 0x4AE90A21

main:
  or  r02,r00,0x1000; Esta instrucción pone en "r02" un "0x1000"
  or  r03,r00,0x2000; Esta instrucción pone en "r03" un "0x2000"

  ld.b  r04,r02,0;
  ld.b  r05,r03,0;

  ld.h  r06,r02,0;
  ld.h  r07,r03,0;

  ld.bu r08,r02,0;
  ld.bu r09,r03,0;

  ld.hu r10,r02,0;
  ld.hu r11,r03,0;

  stop;
```

Se recomienda ejecutar este ejemplo completo y entender los valores que toman los registros `r08`, `r09`, `r10` y `r11` en comparación con los otros 4.

### Un poco de filosofía...

La memoria está dividida en palabras y cada palabra ocupa 4 bytes (32 bits). Un registro también tiene un tamaño de 4 bytes. Para hacer calculos, necesitamos los valores en registros. Sin embargo, a la hora de guardar los valores en memoria, a veces podemos aprovechar una palabra para almacenar más de una variable siempre y cuando no olvidemos de qué tipo es y llevemos un control de ello.

En un byte podemos almacenar valores desde el 00 hasta el FF, es decir, 256 valores distintos.

* Sin signo (todos positivos), los números serían del 0 al 255.
* Con signo (en CA2 o exceso 128), los números irían del -127 al 128.

Dicho de otro modo, dependiendo del tipo, por ejemplo, el valor `0xCA` puede ser 202 (sin signo) o -54 (en CA2).

Así que si en una determinada dirección tenemos almacenado el byte `0xCA`

* haciendo `ld.b`  leeríamos `0xFFFFFFCA` que es -54
* haciendo `ld.bu` leeríamos `0x000000CA` que es 202

Sin embargo, si hacemos `st.b` en ambos casos, se almacena `0xCA`. Por tanto, es nuestra responsabilidad recordar que ese valor está con signo o sin él porque el valor almacenado es exactamente el mismo.

## Instrucciones lógicas

Son las instrucciones `and`, `or` y `xor`. Tienen dos sintaxis.

La primera es esta:

```
and  rDD,rS1,rS2;
or   rDD,rS1,rS2;
xor  rDD,rS1,rS2;
```

En donde los tres operandos son **registros valores**.

* `rDD` es el registro en donde se almacena el resultado.
* `rS1` y `rS2` son los operandos.

Las tres instrucciones admiten una opción `.c` que invierte (niega) el operando `rS2`.

El procesador mc88110 no tiene ninguna operación "mover" o "copiar". Por ello, lo aconsejable es recurrir a estas operaciones para mover un valor de un registro a otro.

Las cuatro instrucciones siguientes son posibles maneras de copiar el valor del registro `rSS` en el registro `rDD`.

```
or  rDD,rSS,rSS;
or  rDD,rSS,r00;
and rDD,rSS,r00;
xor rDD,rSS,r00;
```

> **[STOP]** Antes de avanzar se recomienda recordar las tablas de la verdad de las operaciones `and`, `or` y `xor` lógicas y probar a hacer las operaciones anteriores a mano para comprobar la veracidad de los resultados. No en vano, se ha recomendado al principio, leer este libro **con calma**.

Y por supuesto, al ser conmutativas las operaciones, también vale:

```
or  rDD,r00,rSS;
and rDD,r00,rSS;
xor rDD,r00,rSS;
```

La segunda sintaxis es esta otra:

```
and  rDD,rS1,IMM16
or   rDD,rS1,IMM16
xor  rDD,rS1,IMM16
```

En donde los dos primeros operandos son **registros valores**; el tercero es un valor inmediato de un máximo de 16 bits.

* `rDD` es el registro en donde se almacena el resultado.
* `rS1` e `IMM16` son los operandos.

Nótese que un registro tiene un tamaño de 32 bits y que `IMM16` es de 16 bits (medio registro). Por defecto, el resultado de la operación se almacena en la parte baja (bits menos significativos) de `rDD`.

###### Ejemplos

```
org  0x1000
data 0x52F7AC9B

main:
  ld  r03,r00,0x1000; Carga la dirección 0 + 0x1000
  and r02,r03,0x45EE;

  stop;
```

En el ejemplo anterior, `r03` tiene el valor `0x52F7AC9B` y se ejecuta la siguiente instrucción.

```
and  r02,r03,0x45EE;
```

Así se opera:

```
52F7 AC9B = 0101 0010 1111 0111 1010 1100 1001 1011
     45EE =                     0100 0101 1110 1110

      r02 = ???? ???? ???? ???? 0000 0100 1000 1010
```

Las `?` indican que en esas posiciones estarán los valores que tenía el registro **antes** de hacer la operacion.

De hecho, podemos comprobar que los valores restantes se mantienen. Para ello, vamos a llenar el registro r02 con "basura"

```
org  0x1000
data 0x52F7AC9B

org  0x2000
data 0x12345678

main:
  ld  r03,r00,0x1000; Carga la dirección 0 + 0x1000
  ld  r02,r00,0x2000; Carga la dirección 0 + 0x2000
  and r02,r03,0x45EE;

  stop;
```

> **[CONSEJO]** Este código muestra cómo modificar un ejemplo para hacer otras pruebas. Es altamente recomendable que el lector toquetee el código para entenderlo satisfactoriamente.

Si lo que queremos es almacenar el resultado en la otra mitad del registro, usamos el sufijo `.u`. 

**Importante.** No confundir el `.u` (unsigned, sin signo) de las instrucciones `ld/st` con el `.u` (upper, parte alta) de las instrucciones `and/or/xor`.

###### Ejemplo

```
org  0x1000
data 0x52F7AC9B

org  0x2000
data 0x12345678

main:
  ld    r03,r00,0x1000; Carga la dirección 0 + 0x1000
  ld    r02,r00,0x2000; Carga la dirección 0 + 0x2000
  and.u r02,r03,0x45EE;

  stop;
```

La única diferencia es que esta vez ejecutamos esta instrucción

```
and.u r02,r03,0x45EE;
```

Veamos el desarrollo

```
52F7 AC9B = 0101 0010 1111 0111 1010 1100 1001 1011
     45EE = 0100 0101 1110 1110

      r02 = 0100 0000 1110 0110 ???? ???? ???? ????
```

¿Y qué ocurrirá si ejecutamos **ambas instrucciones** secuencialmente...?

```
and   r02,r03,0x45EE;
and.u r02,r03,0x45EE;
```

