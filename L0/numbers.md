# Números

Una variable es de tipo numérico si almacena en ella un número. Estos son los tipos numéricos:

* int
* double
* char

Pero también:

* byte
* short
* long
* float

Nos centraremos únicamente en `int`, `double` y `char`. A grandes rasgos, `int` es el tipo de los números enteros, `double` el de números reales y `char` el de carácter.

Llamaremos **literal** a los valores "tal cual". Por ejemplo: `3`, `2.5`, `'a'` son ejemplos de literales `int`, `double` y `char` respectivamente.

## El tipo char

El tipo `char` es un poco peculiar porque en realidad *casi* equivale a un tipo `int`. Un tipo char puede tomar valores desde el 0 hasta el 65535 (inclusive). Cada uno de esos números equivale a una letra.

A un tipo `char` podemos asignarle un literal carácter o un literal número. Estos ejemplos serían asignaciones válidas.

```
char myLetter1 = 'A';
char myLetter2 = 65;
```

## Operaciones

En matemáticas, cuando vemos una expresión como esta: `3 + 4`, llamamos:

* **operador** al `+`
* **operando** al `3` y al `4`
* **resultado de la operación** al `7`

Además, el operador decimos que es:

* **numérico** porque el **resultado de la operación** es un número.
* **binario** porque opera sobre **dos operandos**.
* **infijo** porque el operador va **en medio** de los dos operandos.
* **sus operandos** son números

Del mismo modo, los operadores `-`, `*`, `/` y `%` son operadores **numéricos, binarios, infijos de operandos numéricos**.

Recordemos que los tipos `char` e `int` son equivalentes. Por ello, estas operaciones son correctas:

```
int strangeNumber1 = 'a' * 3;
int strangeNumber2 = 'b' - 1;
```

El manejo de números en Java es sencillo pero tiene dos inconvenientes: la conversión de tipos y las operaciones con tipos.

### Operaciones con tipos

> **IMPORTANTE**. Un número sin decimales (`1`, `3`,...) es un número `int`. Un número con decimales (aunque sean todos ceros) es un número `double` (`1.5`, `2.0`,...)

Vamos a ver qué ocurre con las operaciones matemáticas según los operandos sean del mismo tipo o distinto.

**Si ambos operandos son del mismo tipo**, el resultado será del mismo tipo. Veamos un ejemplo en el que esto es problemático.

Queremos hacer la división `1 / 2`. Al hacer esta división, como `1` y `2` son **enteros**, el resultado es un entero, es decir, `0`. Observemos este resultado

```
int zero = 1/2 * 2;
```

Java opera en este orden:

1. Hace la división. `1/2`. Como ambos son enteros, el resultado debe ser entero. `1/2` vale `0`.
2. Hace la multiplicación. `0*2`. Como ambos son enteros, el resultado debe ser entero. `0*2` vale `0`.
3. Hace la asignación. A la variable `zero` le asigna el valor `0`.

En ensamblador:

```
LEA(r02, 1)     ; zero(r02) <- 1
LEA(r03, 2)     ; 2 (r03)
div  r02,r02,r03; zero(r02) <- 0 <- 1 / 2

LEA(r03, 2)     ; 2 (r03)
muls r02,r02,r03; zero(r02) <- 0 <- 0 * 2
```

**Si ambos operandos son de distinto tipo**, el resultado será del tipo de *mayor tamaño*. El tamaño de las variables sigue este orden

double &lt; float &lt; long &lt; int &lt; char &lt; short &lt; byte

(donde `double` es el tamaño mayor y `byte` el menor)

Veamos un ejemplo equivalente al anterior. Vamos a hacer la división `1 / 2.0`. Observa que el denominador es de tipo `double`. Como son de tipos distintos, el resultado es del tipo de tamaño mayor, `double`. Luego el resultado es `0.5`.

Observemos ahora qué ocurre al encadenar operaciones

```
double one = 1/2.0 * 2;
```

1. Hace la división. `1/2.0`. Al ser de diferente tipo, el resultado debe ser del de mayor tamaño, es decir `double`. `1/2.0` vale `0.5`.
2. Hace la multiplicación. `0.5*2`. Al ser de diferente tipo, el resultado debe ser el de mayor tamaño, es decir, `double`. `0.5*2` vale `1.0`.
3. Hace la asignación. A la variable `one` le asigna el valor `1.0`.

### Conversión de tipos

En este ejemplo estamos metiendo el valor de un entero en un `double`:

```
int myInt = 8;
double myDouble = myInt;
```

Al hecho anterior, lo llamamos **conversión de tipo** de `int` a `double`.

En general, podemos convertir una variable de un tipo de tamaño menor a uno de tamaño mayor. Es decir, si los tamaños se ordenan así:

double &lt; float &lt; long &lt; char &lt; int &lt; short &lt; byte

Podemos convertir de derecha a izquierda.

La conversión del primer ejemplo sí sería válida porque `int` es de tamaño menor que `double`. Sin embargo esta otra daría error:

```
double pi = 3.14;
char myLetter = pi;
```

Sin embargo, existe una manera de forzar que las conversiones como esta última funcionen y es utilizando el operador **cast**

## Operador cast

Es un operador

* **unario** porque opera sobre **un operando**.
* **prefijo** porque el operador va **al principio** del operando.
* **su operando** es una variable o un valor

Un ejemplo lo aclara todo:

```
double pi = 3.14;
char myLetter = (char)pi;
```

Lo que hacemos es forzar la conversión de la variable `pi` en una variable de tipo `char`. Lo que hará Java es truncar (quitar los decimales) para asignar a `myLetter` el valor `3`.

Nota. Aunque para pasar de `int` a `double` no haga falta el operador cast, se recomienda ponerlo siempre

## Precedencia de operadores

Por matemáticas sabemos que la multiplicación y división **preceden a** (se operan antes que) la suma y la resta.

El operador **cast** precede a los 5 operadores numéricos. Veámoslo con dos ejemplos

```
(double)1/(double)2 * 2;
```

Aquí el orden es este:

1. Operador cast: `1` pasa a ser `1.0`
2. Operador cast: `2` pasa a ser `2.0`
3. División: `1.0/2.0` es igual a `0.5`
4. Multiplicación: `0.5 * 2` es igual a `1.0`

Sin embargo, si hacemos esto:

```
(double)(1/2) * 2;
```

El orden queda así:

1. División: `1/2` es igual a `0` (división entera)
2. Operador cast: `0` pasa a ser `0.0`
3. Multiplicación: `0.0 * 2` es igual a `0.0`

---
