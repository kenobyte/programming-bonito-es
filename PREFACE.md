# Programar vs programar bonito

Con este libro no pretendo que tú, futuro lector, aprendas a programar. De hecho, es un error creer que programar es aprender a **picar código**. A programar se aprende programando y es igual qué lenguaje de programación cojas. Para programar bien puedes usar el Libro de Knuth [^1] o puedes simplemente plantearte problemas para que vayas cogiendo el tranquillo a esto de programar.

En este libro lo que sí pretendo es que, sepas **programar bonito**, es decir, que además de saber resolver un problema mediante un lenguaje de programación, sepas si esa solución es la más adecuada o no. Por ello, aunque tratemos conceptos básicos de la programación, profundizaremos mucho en ellos.

En este libro vamos a programar usando Java, uno de los lenguajes más modernos y más utilizados hoy día. En concreto usaremos la versión 7 de Java.

Sin embargo, también usaremos un lenguaje máquina (ensamblador), lo cual para un programador novicio o un estudiante puede ser sinónimo de **tortura**. Pero la intención es más que buena. El propósito de usar un lenguaje máquina es entender cómo se comporta el compilador de Java. Si bien no es exacta la transcripción que haremos, nos acercaremos mucho. Si se entiende *cómo* el ordenador procesa lo que ponemos en Java, podemos entender *por qué* un código es mejor que otro. Además, mientras escribía este libro me di cuenta de que si se entiende qué relación hay entre ambos lenguajes, se entiende mejor ensamblador.

El lenguaje máquina escogido es el del procesador mc88110 pero podría haber sido cualquiera. De hecho, lo ideal quizá hubiera sido coger un lenguaje máquina ficticio pero eso implicaría tener que describir el lenguaje y la máquina ficticias. Además, eso ya lo ha hecho Knuth. La decisión por este procesador es porque es el que conozco. Y porque a mi círculo más cercano creo que le será más útil que lo haga en ello.

## A dónde ir

1. Si no has programado nunca en tu vida y buscas un tutorial para diseñar tu aplicación de Android en 30 minutos, cierra el libro.
2. Si no te suenan las palabras *registro*, *memoria*, *instrucción*, cierra el libro.
3. En caso contrario, continúa.

## Notas importantes

* Este libro se debe leer con calma. Mucha calma. Se pone mucha información y cada vez más compleja. Y cada vez que se pone un texto se da por hecho que lo anterior se ha entendido.
* Los ejemplos están para probarlos. No es coña. Saber crear tu propio programa y saber probarlo es fundamental.
* Programar no es fácil. Puede hasta ser frustrante. No lo negaremos.
* Cada vez que salga el nombre de [Dijkstra], chupito. Si ha salido su nombre porque usamos un algoritmo suyo, dos chupitos.

[^1]: Knuth, Donald. *The Art of Computer Programming*. ISBN: 0-201-03801-3.

[Dijkstra]: http://es.wikipedia.org/wiki/Edsger_Dijkstra "Dijkstra"
