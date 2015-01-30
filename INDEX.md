# Índice

Los capítulos marcados con un asterisco \* utilizan lenguaje ensamblador.

## A. El lenguaje ensamblador mc88110

Este capítulo sirve de iniciación al lenguaje ensamblador 88110.

1. \* **Procesador mc88110**  
   Características del procesador 88110. Modos de redireccionamiento, juego de instrucciones, etc. propias de este procesador

2. \* **Convenciones propias**  
   Convenciones que se van a seguir en este libro. Segmentos de memoria, uso específico de registros.


## Level 0

Programación secuencial

1. **Tipos numéricos**  
   Cómo se manejan números en Java. De qué tipos pueden ser las variables numéricas, qué valores pueden tener.

   \* **Carga de variables numéricas en ensamblador**  
   Macros LEA.

   \* **Pila de Operaciones**  
   Cómo usar la pila para hacer operaciones. Notación polaca inversa.

2. **Booleanos**  
   Qué son los tipos booleanos. Cuáles son los operadores booleanos y cuál es la precedencia respecto a los operadores numéricos. Lazy calculation

   \* **Operaciones booleanas en ensamblador**  
   Instrucción cmp. Operaciones AND y OR lógicas convertidas en saltos condicionales.

3. **Estructura de control if-else**  
   Qué es la estructura de control if-else. Deficiencias de código habituales

   \* **Salto condicional**  
   Instrucciones bb0 y bb1 en ensamblador

4. **Reglas de estilo**  
   Consejos a la hora de escribir código en Java. Convenciones en nombres de variables. Espacios y tabulaciones.
   
5. Test


## Level 1

Flujos de instrucciones no secuenciales

1. **Métodos externos**  
   Qué son y cómo se definen. Paso de parámetros por copia.

   \* **Pila como almacén de variables locales**  
   Cómo usar la pila para almacenar variables y pasar parámetros

2. **Arrays**  
   Paso por copia de puntero.

   \* **Heap de Memoria**  
   Introducción a la memoria dinámica

3. **Bucles**  
   Bucles while, do-while y for. Buenas prácticas.

   \* **Stack overflow**  
   Excepción de desbordamiento de pila como conscuencia de bucles infinitos.

6. Test

## Level 2

Recursividad

1. **Recursividad**  
   Qué es la recursividad. Ejemplos de uso.

2. Test

---


## Level 3

Introducción a la Programación Orientada a Objetos

1. **Definiciones de Clase, objeto e interfaz**  
   Declaración e implementación de una clase. Visbilidad de clases. Uso de objetos.

   \* **Heap de Memoria II**  
   Strings en ensamblador. Garbage collector

2. **Clases Integer, Double, Boolean...**  
   Valor vs referencia. Paso de parámetros

3. **Excepciones**  
   Manejo de excepciones. Tipos de excepciones. Excepciones propias.

4. Ejercicio práctico.

## Level 3-j

Algunas cosas importantes en Java.

1. Método equals
2. Método toString
3. Interfaces Comparable, Comparator
4. Clase Math

## Level 4

Introducción a las Tipos Abstractos de Datos.

1. **Introducción a clases genéricas**  
   Qué es una clase genérica. Box-model. Cuándo se usa
   
2. **Interfaz lista, conjunto, pila y cola**

## Level 5

Implementación de Tipos Abstractos de Datos

1. **Implementaciones basadas en vectores**

2. **Implementaciones basadas en nodos enlazados**

3. Iteradores

## Level 6

* Box-model
* PriorityQueue & Heap
* Tree & BinaryTree

-------------

## Level W

* For-each
* Operador ternario

## Level X

* Enumerados
* Genéricos I
* Genéricos II
* Operadores II
