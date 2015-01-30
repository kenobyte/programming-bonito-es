# Características generales

Este capítulo trata sobre la sintaxis del lenguaje máquina del procesador Motorola 88110.

Para continuar con este capítulo es imprescindible saber qué es un procesador, un registro, qué es la memoria principal, qué es un lenguaje máquina, qué son los modos de direccionamiento, etc.

## Forma de una instrucción.

Toda instrucción tiene la siguiente sintaxis:

```
[ETIQUETA :] INSTRUCCIÓN[.OPCIÓN]  ARG0, ARG1, ARG2; [COMENTARIO]
```

Los corchetes indican que es opcional. Los demás signos de puntuación son obligatorios.

* `ETIQUETA` es una cadena alfanumérica que **identifica una instrucción**. Sirve para referirse a una determinada "línea" del código sin tener que recordar el número de dicha línea.
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

* Valor inmediato de hasta 16 bits. Por ejemplo `3`.
* Registro. Por ejemplo `r03`.
* Campo de bits, en donde W5 es el tamaño y O5 el origen del campo de bits.

  Por ejemplo, `04<20>` significa un campo de bits *desde* el bit 20 con un *tamaño* de 4. Es decir, si tenemos la siguiente cadena:

  ```
  01010001010101001010101001000101
  ```

  El campo de bits `04<20>` de dicha cadena es el señalado con asteriscos:

  ```
            ****
  0101 0001 0101 0100   1010 1010 0100 0101
               ^    ^                     ^
               20   16                    00
  ```

  (Los números de abajo indican la posición)