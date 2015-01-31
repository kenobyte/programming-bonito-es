# Características generales

Este capítulo trata sobre la sintaxis del lenguaje máquina del procesador Motorola 88110.

Para continuar con este capítulo es imprescindible saber qué es un procesador, un registro, qué es la memoria principal, qué es un lenguaje máquina, qué son los modos de direccionamiento, etc.

## Formato de una instrucción.

Toda instrucción tiene la siguiente sintaxis:

```
[ETIQUETA :] INSTRUCCIÓN[.OPCIÓN]  ARG0, ARG1, ARG2; [COMENTARIO]
```

Los corchetes indican que es opcional. Los demás signos de puntuación son obligatorios.

* `ETIQUETA` es una cadena alfanumérica que identifica una instrucción. Sirve para referirse a una determinada "línea" del código sin tener que recordar el número de dicha línea.
* `INSTRUCCIÓN` es el mnemónico de la instrucción.
* `OPCIÓN` es una cadena que hace que la instrucción se comporte de manera diferente.
* `ARG0`, `ARG1` y `ARG2` son los argumentos de la instrucción.
* `COMENTARIO` es un comentario.

Estos son ejemplos de instrucciones:

```
FIRST_LINE:
    and  r03,r03,r02; Esto es un and
    or.c r05,r07,712;
    cmp  r04,r03,5  ;
```

**IMPORTANTE.** la etiqueta `FIRST_LINE` es etiqueta de la instrucción `and`. No del bloque completo.

**IMPORTANTE.** una instrucción puede llevar como máximo **una etiqueta**. El siguiente código no es válido:

```
FIRST_LINE:
SECOND:
    ld  r02,r03,r00;
```

## Argumentos de una instrucción

Los tipos de argumentos que puede tener una instrucción son:

* **Valor inmediato de hasta 16 bits.**

  Puede ser en hexadecimal (por ejemplo `0x3F3`) o en decimal (por ejemplo `3`).

* **Registro.**

  Comienza por la letra `r`. Por ejemplo: `r03`

* **Campo de bits** de la forma `W5<05>`, en donde W5 es el tamaño y O5 el origen del campo de bits.

  Por ejemplo, `04<20>` significa un campo de bits *desde* el bit 20 con un *tamaño* de 4. Es decir, si tenemos la siguiente cadena:

  ```
  01010001010101001010101001000101
  ```

  El campo de bits `04<20>` de dicha cadena es el señalado con asteriscos (los números de abajo indican la posición de cada bit):

  ```
            ****
  0101 0001 0101 0100   1010 1010 0100 0101
               ^    ^                     ^
               20   16                    00
  ```

## Pseudo-instrucciones

Son instrucciones de ensamblador que no ejecutan programa. Indican *cómo* se debe generar el código.

**org.** Indica en qué dirección de memoria está situada la siguiente instrucción. Por ejemplo, el código siguiente indica que la instrucción "and" está en la dirección 0x1000

```
org 0x1000
and r02,r00,1;
```

**data.** Reserva e inicializa un conjunto de *palabras* (sectores de 4 bytes) en memoria con unos datos. La sintaxis es la siguiente:

```
data NÚMERO [, NUMERO][, NUMERO][, NUMERO]
data "CADENA" [, "CADENA"][, "CADENA"]...
```

El número puede ser en decimal o en hexadecimal. Por ejemplo:

```
org  0x1000
data -1, 2, 3
data 0x2FFF
```

En la instrucción anterior se reservan las siguientes direcciones de memoria y datos:

* Dirección 0x1000 a 0x1003. Dato `0xFFFFFFFF` (-1)
* Dirección 0x1004 a 0x1007. Dato `0x00000002` (2)
* Dirección 0x1008 a 0x100B. Dato `0x00000003` (3)
* Dirección 0x100C a 0x100F. Dato `0x00002FFF` (0x2FFF)

También se pueden inicializar cadenas de caracteres. Por ejemplo:

```
org  0x2000
data "Hola Mundo\n"
```

Almacena en memoria:

* Dirección 0x2000 a 0x2003. Dato `0x486F6C61` (Caracteres 'H', 'o', 'l', 'a')
* Dirección 0x2004 a 0x2007. Dato `0x204D756E` (Caracteres ' ', 'M', 'u', 'n')
* Dirección 0x2008 a 0x200B. Dato `0X646F0A00` (Caracteres 'd', 'o', '\n')

