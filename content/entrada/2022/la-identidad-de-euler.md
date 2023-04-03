---
title: La identidad de Euler
description: ""
date: 2022-06-28T23:02:42.099Z
preview: ""
draft: ""
categories: matemáticas
lastmod: 2022-08-28T03:59:58.033Z
---

## La fórmula más notable en matemáticas

En una de sus famosas conferencias de Física, Richard Feynman le dedica una clase completa a "una de las fórmulas más notables, casi asombrosas, de todas las matemáticas." Al comenzar, Feynman explica que "desde el punto de vista del físico, podríamos presentar esta fórmula en dos minutos más o menos, y terminar con ella. Pero la ciencia es tanto para el disfrute intelectual como para la utilidad práctica, así que en lugar de dedicar unos minutos a esta asombrosa joya, rodearemos la joya con su engaste adecuado en el gran diseño de esa rama de las matemáticas que se llama álgebra elemental." Un poco antes de culminar, la llama además "la fórmula más notable en matemáticas".

La formula a la que Feynman se refiere es la famosa fórmula de Euler que su autor Leonhard  Euler popularizó en el capítulo 8 del *Introductio in analysin infinitorum* o Introducción al análisis del infinito. Euler sentó las bases del análisis matemático con su *Introductio*, que se ha llegado a comparar con los *Elementos* de Euclides por su significado para las matemáticas. La fórmula, como nos dice Feynman, es la siguiente:

<p style="text-align: center;">
e<sup>i&#952;</sup> = cos &#952; + i sen &#952;
</p>

Contemplemos sus partes:

## El número *e*

El número *e* es un número irracional y trascendente. El valor de *e* reducido a sus primeras cifras decimales es el siguiente:
*e* = 2,71828182...Esta constante aparece en la computación de interés compuesto cada vez que se aplica un cambio extremadamente pequeño un número extremadamente grande de veces.

Como se sabe, la fórmula para computar el interés compuesto es la siguiente:

x = C(1+t/n)<sup>nu</sup> - C

dónde:

x = interés compuesto + capital.

C = capital (el monto del depósito inicial o del préstamo).

t = tasa de interés.

n = número de períodos de capitalización.

u = número de unidades de tiempo en que el dinero se invierte o se solicita en préstamo.

Así que si comenzamon con un capital de $1000 dólares, con una tasa de interés anual digamos de 100%, entonces:

x = C(1+t/n)<sup>nu</sup> - C

x = 1000 (1+1/1)<sup>(1)(1)</sup>

x = 1000 (1+1/1)<sup>(1)</sup>

x = 1000 (2)<sup>(1)</sup>

x = 1000 (2)

x = 2000

En un año tendríamos en nuestro generoso banco $1000 de interés compuesto acumulado más el capital original, para un total de 2000 dólares.

Hagamos una tabla. Disminuyamos la tasa de interés anual a un % consecutivamente más ínfimo y dejemos el período de capitalización en su forma anual:

| t     | (1+t/n)<sup>(n)(u)</sup> |  x  |
| ----------- | ----------- | ----------- |
| 1     |(1+1/1)<sup>(1)(1)</sup> |   $2000|
| 0.5  |   (1+.5/1)<sup>(1)(1)</sup>   | $1500|
| :      | :      |  :  |
| 0.1   | (1+.1/1)<sup>(1)(1)</sup>     | $1100|
| 0.01   | (1+.01/1)<sup>(1)(1)</sup>    | $1010|

Por supuesto sabemos que a medida que disminuye la tasa de interés anual, disminuye igualmente el monto de la ganancia. Que tal si dejamos la tasa de interés al 10% y cambiamos el número de períodos de capitalización, mensualmente, 12 veces al año:

| t     | (1+t/n)<sup>(n)(u)</sup> |  x  |
| ----------- | ----------- | ----------- |
| 0.1     |(1+.1/12)<sup>(12)(1)</sup> |   1104.71|

Hagamos algo diferente. Esta vez cambiémos dos variables en vez de una. Disminuyamos paulativamente la tasa de interés mientras aumentamos el período de capitalización:

| t     | (1+t/n)<sup>(n)(u)</sup> |  x  |
| ----------- | ----------- | ----------- |
| 1     |(1+1/1)<sup>(1)(1)</sup> |   $2000|
| 0.5  |   (1+.5/2)<sup>(2)(1)</sup>   | $1562.50|
| 0.4      |(1+.4/4)<sup>(4)(1)     |  $1464.10 |
| 0.3   | (1+.3/6)<sup>(6)(1)</sup>     | $1322.50|
| 0.2   | (1+.2/12)<sup>(1)(1)</sup>    | $1219.31|

Lo anterior lo hemos hecho para observar la funcionalidad de la fórmula para computar el interés compuesto.

Enfoquémonos ahora en la parte de la fórmula que nos ayuda a calcular la ganancia (1+1/n)<sup>nu</sup>. Dejemos que *y* en este caso sea la ganancia en forma decimal por la que multiplicaríamos nuestro capital e imaginémonos una cierta cantidad inmensa de veces que se realizaría la capitalización en un año con un interés anual de 100%. Extendamos los dígitos de ***y*** a 10 lugares solamente:

| n   | (1+t/n)<sup>(n)(u)</sup> |  y en decimal |
| ----------- | ----------- | ----------- |
| 1  | (1+1/1)<sup>(1)(1)</sup>    |2,00000000 |
| 5   |(1+1/5)<sup>(5)(1)</sup>    | 2,48832000|
| 10   |(1+1/10)<sup>(10)(1)</sup>    |2,59374246601|
| 100  |(1+1/100)<sup>(100)(1)</sup>    | 2,7048138294|
| 1000 |(1+1/1000)<sup>(1000)(1)</sup>    |2,7169239322 |
| 10000 | (1+1/10000)<sup>(10000)(1)</sup>    |2,7181459268|
| 100000  | (1+1/100000)<sup>(100000)(1)</sup>    |2,7182682371|
| 1000000   | (1+1/10000000)<sup>(1000000)(1)</sup>    | 2,7182804693|
| 10000000   | (1+1/100000000)<sup>(10000000)(1)</sup>    | 2,7182816925|
| 100000000   | (1+1/1000000000)<sup>(100000000)(1)</sup>    | 2,7182818149|
| 1000000000   | (1+1/1000000000)<sup>(1000000000)(1)</sup>    |2,7182818271 |
| 10000000000   | (1+1/10000000000)<sup>(10000000000)(1)</sup>    |2,7182818283 |

Si la capitalización ocurre diez mil millones de veces en un año a un interés de un 100% obtendríamos una ganancia equivalente al producto de nuestro capital y 2,7182818283.

Este número es una aproximación a ***e***, el mismo que en nuestra fórmula inicial tan alabada por Feynman:

<p style="text-align: center;">
e<sup>i&#952;</sup> = cos &#952; + i sen &#952;
</p>

Por supuesto que este tipo de capitalización contínua no existe en la realidad, ya que la mayoría de los intereses se capitalizan diaria, semanal, mensual, trimestral, semestral o anualmente. Pero resulta interesante imaginarse o generalizar que sucedería si pudiéramos continuar de esta manera.

El número ***e*** no solamente es interesante por estas propiedades.

***e*** es un número límite. No importa si capitalizamos *ad infinitum*, nunca avanzaríamos más que el producto de nuestro capital y este número prodigioso.

***e***, el número de Euler, puede ser por la tanto definido como:

El límite de (1+t/n)<sup>(n)(u)</sup> mientras n continúa al infinito.

<div>\begin{align} e=\lim_{n\rightarrow\infty}\left( 1+\frac{1}{n}\right)^n\end{align}</div>

El número ***e*** no fue inventado por Euler -se conocía desde antes- fue Euler sin embargo quien, por primera vez, usó la función exponencial en vez del logaritmo en su formulación de ***e***. Esta constante ***e*** se usa por lo tanto como la base común para las tasas de crecimiento, por lo que juega un papel fundamental en la descripción y predicción de fenómenos naturales que varían exponencialmente con el tiempo, desde la inversión financiera pasando por la tasa de propagación de enfermedades hasta la desintegración radiactiva. Cuando n representa el tiempo, ***e*** significa una tasa de crecimiento (o disminución, para n negativo) que es igual al tamaño o cantidad que se ha acumulado hasta el momento.

Entonces, en ese sentido, el número ***e*** es fundamental en la física, la ingeniería y las ciencias económicas. ***e*** representa la idea de que todos los sistemas en continuo crecimiento son versiones escaladas de una tasa común.

## Otras definición de ***e*** usando factoriales

<div>\begin{align} e=\lim_{n\rightarrow\infty}\left( 1+\frac{1}{n}\right)^n=1+\frac{1}{1}+\frac{1}{1\cdot2}+\frac{1}{1\cdot3}+\frac{1}{1\cdot4}...\end{align}</div>

Lo cual es lo mismo que:

<div>\begin{align} e=\lim_{n\rightarrow\infty}\left( 1+\frac{1}{n}\right)^n=1+\frac{1}{1!}+\frac{1}{2!}+\frac{1}{3!}+\frac{1}{4!}...\end{align}</div>

Ahora llegamos a otro número en la fórmula que, a diferencia de ***e***, no existe en la realidad, lo cual no significa que no sea importante. Pero no nos detendremos en él, ni en los próximos, no más que para definirlos.

## La unidad de número imaginario ***i***

Pero, mientras hemos visto una aplicación de (1+t/n)<sup>(n)(u)</sup>, ¿de dónde sale ***i***?

Sabemos que ***i***<sup>2</sup> = -1, lo cual quiere decir que,

La ***i*** es la unidad de número imaginario cuya definición es:

***i***<sup>2</sup> = -1

<div> i = $\sqrt[]{-1}$ </div>

## La letra &#952; (theta), en geometría, representa un ángulo cualquiera.

Es como la ***x*** en álgebra que podemos sustituir por un número cualquiera.

## *cos* y *sen*...

se refieren por supuesto a las funciones trigonométricas coseno y seno respectivamente.

En matemáticas, el seno y el coseno son funciones trigonométricas de un ángulo. El seno y el coseno de un ángulo agudo se definen en el contexto de un triángulo rectángulo: para el ángulo especificado, su seno es la relación entre la longitud del lado opuesto a ese ángulo y la longitud del lado más largo del triángulo (la hipotenusa), y el coseno es la razón de la longitud del cateto adyacente a la de la hipotenusa.

<img src="/img/euler/Triangulo-Sen-Cos.svg" alt="Triángulo: Seno y Coseno" width="50%"/>

Valdría la pena en este momento escribir con palabras lo que dice esta fórmula en castellano antes de proceder, para volver a apreciar en su conjunto la joya que es:

Dice:

*El límite de (1+t/n)<sup>(n)(u)</sup> mientras n continúa al infinito elevado al poder del producto del número imaginario y un ángulo es igual a la suma del coseno del ángulo más el producto del número imaginario y el seno del ángulo.*

<p style="text-align: center;">
e<sup>i&#952;</sup> = cos &#952; + i sen &#952;
</p>

¡Vaya ocurrencia! Y este no es el final.

Por alguna razón, que va más allá del alcance de este artículo, Euler decidió sustituir el valor del ángulo &#952; por &#960;, otro número trascendental e irracional. Con ello extendió el reino de la trigonometría.

La palabra trigonometría proviene del griego τριγωνο trigōno o triángulo y μετρον metron o medida. ¿Pero que tal si aplicamos sus principios al círculo, como hizo Euler? A fin de cuentas, si nos fijamos en la siguiente figura podemos percibir la relación entre la geometría del círculo y la trigonometría. Adicionemos además coordenadas:

<img src="/img/euler/Trigonometria.webp" alt="Triángulo y Círculo: Seno y Coseno" width="80%"/>

El resultado, entonces, cuando &#952; = &#960;:

<p style="text-align: center;">
e<sup>i&#952;</sup> = cos &#952; + i sen &#952;
</p>

<p style="text-align: center;">
e<sup>i&#960;</sup> = cos &#960; + i sen &#960;
</p>

## Otra definición del coseno y seno. 

Dado un punto en el círculo unitario, 
en contra del sentido de las agujas del reloj, desde el eje x positivo,
cos &#952; es la coordenada x del punto,
sen &#952; es la coordenada y del punto.

Entonces obtenemos la identidad de Euler a partir de la fórmula de Euler y enviando - 1 al lado derecho:

<p style="text-align: center;">
e<sup>i&#960;</sup> = (-1) + (0)
</p>

<p style="text-align: center;">
e<sup>i&#960;</sup> = -1
</p>

<p style="text-align: center;">
e<sup>i&#960;</sup>+1 = 0
</p>

Y esta es la famosa **identidad de Euler**. Que dice:

*La constante Euler, elevada al poder del producto del número imaginario **i** y **&#960;** más 1, es igual a 0.* Confluyen las constantes más importantes de las matemáticas: e, i, &#960;, 1 y 0, al igual que varias ramas de esta como el álgebra, la trigonometría, la geometría y el cálculo. Extraordinaria joya, y como decía Feynman, la fórmula más notable de las matemáticas.

Continuará...

<script type="text/javascript"
  src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML">
</script>
<script type="text/x-mathjax-config">
MathJax.Hub.Config({
  tex2jax: {
    inlineMath: [['$','$'], ['\\(','\\)']],
    displayMath: [['$$','$$'], ['\[','\]']],
    processEscapes: true,
    processEnvironments: true,
    skipTags: ['script', 'noscript', 'style', 'textarea', 'pre'],
    TeX: { equationNumbers: { autoNumber: "AMS" },
         extensions: ["AMSmath.js", "AMSsymbols.js"] }
  }
});
</script>

<script type="text/x-mathjax-config">
  MathJax.Hub.Queue(function() {
    // Fix <code> tags after MathJax finishes running. This is a
    // hack to overcome a shortcoming of Markdown. Discussion at
    // https://github.com/mojombo/jekyll/issues/199
    var all = MathJax.Hub.getAllJax(), i;
    for(i = 0; i < all.length; i += 1) {
        all[i].SourceElement().parentNode.className += ' has-jax';
    }
});
</script>
<style type="text/css">
    ol { list-style-type: lower-alpha; }
</style>
<style type="text/css">
code.has-jax {font: inherit;
              font-size: 100%;
              background: inherit;
              border: inherit;
              color: #515151;}
</style>
