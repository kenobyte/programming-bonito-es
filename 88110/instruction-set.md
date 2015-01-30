# Juego de Instrucciones

Se van a detallar todas las instrucciones disponibles del lenguaje ensamblador del procesador mc88110. Se dividen en las siguientes categorías. Adicionalmente, se van a incluir algunos ejemplos de uso reales para entender la trascendencia de algunas instrucciones y opciones.

1. [Carga/almacenamiento](#store-load)
2. [Lógicas](#logic)
3. [Aritméticas](#arithmetic)
4. [Campo de bit](#bit)
5. [Control de flujo](#flow)

## Instrucciones de carga/almacenamiento <a id="store-load"></a>

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
* `.d` Se tranfiere doble palabra.

En el siguiente esquema se especifica qué parte del registro corresponde el byte y la media palabra.

```   
r02 =  |0100 1010 1110 1001  0000 1010 1010 0001|
                                           |0001| .b
                            |0000 1010 1010 0001| .h
```

La instrucción `ld` con las opciones `.h` y `.b` realiza una extensión de signo. 

Supongamos que en las direcciones de memoria 1000 y 2000 tenemos lo siguiente:

```
#1000 = |0100 1010 1110 1001  1000 1010 1010 1001|
#2000 = |0100 1010 1110 1001  0000 1010 1010 0001|
```

Y que los registros `r03` y `r04` valen `1000` y `2000` respectivamente.

Ahora, llamamos a `ld` con las diferentes opciones.

`ld.b r02,r03,0`. Almacena el contenido de la dirección 1000+0 en el registro `r02` y realiza extensión de signo.

```
#1000 |---- ---- ---- ----  ---- ---- ---- 1001| .b
r02 = |                                    1001|
r02 = |1111 1111 1111 1111  1111 1111 1111 1001|
```

`ld.b r02,r04,0`. Almacena el contenido de la dirección 2000+0 en el registro `r02` y realiza extensión de signo.

```
#2000 |---- ---- ---- ----  ---- ---- ---- 0001| .b
r02 = |                                    0001|
r02 = |0000 0000 0000 0000  0000 0000 0000 0001|
```

Si no se quiere hacer extensión de signo, se utilizan las siguientes opciones:

* `.bu` Carga un byte sin extender signo
* `.hu` Carga media palabra (la menos significativa) sin extender signo.

### Un poco de filosofía...

La memoria está dividida en palabras y cada palabra ocupa 4 bytes (32 bits). Un registro también tiene un tamaño de 4 bytes. Para hacer calculos, necesitamos los valores en registros. Sin embargo, a la hora de guardar los valores en memoria, a veces podemos aprovechar una palabra para almacenar más de una variable siempre y cuando no olvidemos de qué tipo es y llevemos un control de ello.

En un byte podemos almacenar valores desde el 00 hasta el FF, es decir, 256 valores distintos.

* Sin signo (todos positivos), los números serían del 0 al 255.
* Con signo (en CA2 o exceso 128), los números irían del -127 al 128.

Dicho de otro modo, dependiendo del tipo, por ejemplo, el valor `CA` puede ser 202 (sin signo) o -54 (en CA2).

Así que si en una determinada dirección tenemos almacenado el byte `CA`

* haciendo `ld.b`  leeríamos `FFFFFFCA` que es -54
* haciendo `ld.bu` leeríamos `000000CA` que es 202

Sin embargo, si hacemos `st.b` en ambos casos, se almacena `CA`. Por tanto, es nuestra responsabilidad recordar que ese valor está con signo o sin él porque el valor almacenado es exactamente el mismo.

## Instrucciones lógicas <a id="logic"></a>

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

> **STOP!**. Antes de avanzar se recomienda recordar las tablas de la verdad de las operaciones `and`, `or` y `xor` lógicas y probar a hacer las operaciones anteriores a mano para comprobar la veracidad de los resultados. No en vano, se ha recomendado al principio, leer este libro **con calma**.

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

Por ejemplo, supongamos que `r03` tiene el valor `52F7 AC9B` y hacemos la siguiente instrucción.

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

Si lo que queremos es almacenar el resultado en la otra mitad del registro, usamos el sufijo `.u`. 

**Importante** No confundir el `.u` (unsigned, sin signo) de las instrucciones `ld/st` con el `.u` (upper, parte alta) de las instrucciones `and/or/xor`.

Ejemplo, con los mismos registros, ahora hacemos la siguiente instrucción:

```
and.u r02,r03,0x45EE;
```

Veamos el desarrollo

```
52F7 AC9B = 0101 0010 1111 0111 1010 1100 1001 1011
     45EE = 0100 0101 1110 1110

      r02 = 0100 0000 1110 0110 ???? ???? ???? ????
```

¿Y qué ocurrirá si hacemos ambas cosas secuencialmente?

```
and   r02,r03,0x45EE;
and.u r02,r03,0x45EE;
```

Pues esto:

```
52F7 AC9B = 0101 0010 1111 0111 1010 1100 1001 1011
     45EE =                     0100 0101 1110 1110

      r02 = ???? ???? ???? ???? 0000 0100 1000 1010

52F7 AC9B = 0101 0010 1111 0111 1010 1100 1001 1011
     45EE = 0100 0101 1110 1110

      r02 = 0100 0000 1110 0110 0000 0100 1000 1010
```

**Importante**. Entender este concepto es fundamental para entender después la macro `LEA`.

