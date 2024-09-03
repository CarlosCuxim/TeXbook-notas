# Como $\TeX$ rompe párrafos en líneas

Una de las cualidades de $\TeX$ es que tiene un complejo algoritmo para crear las líneas que conforman los párrafos en un documento. Este permite que hasta una palabra al final del párrafo afecte como $\TeX$ compone las líneas para lograr siempre el mejor resultado.

Sin embargo, esto a veces conduce en rupturas que no son, psicológicamente, adecuadas. Una forma de resolver esto es con una “atadura”, espacios irrompibles que se agregarán mediante el carácter `~`. Algunos ejemplos de su uso son los siguientes
- **En referencias a partes enumeradas de un documento:** 
	- `Chapter~12`
	- `Appendix~A`
	- `Figure~3`
	- `Theorem~1.2`
	- `Table~\hbox{B-8}`
	- `Lemmas 5 and~6`
- **Entre los nombres de una persona o entre múltiples apellidos:**
	- `Donald~E. Knuth`
	- `Bartel~Leendert van~der~Waerden`
	- `Luis~I. Trabb~Pardo`
	- `Charles~XII`
	- `Charles Louis Xavier~Joseph de~la Vall\'ee~Poussin`
- **Entre símbolos matemáticos unidos a sustantivos:**
	- `dimension~$d$`
	- `widht~$w$`
	- `function~$f(x)$`
	- `string~$s$ of lenght~$l$`
	- `string~$s$ of lenght $l$~or more`
- **Entre símbolos en series:**
	- `1,~2, or~3`
	- `$a$,~$b$, and~$c$`
	- `1,~2, \dots,~$n$`
- **Cuando un símbolo está ligado a una preposición:**
	- `of~$x$`
	- `from 0 to~1`
	- `increase $z$ by~1`
	- `in common with~$m$`
	- `of $u$~and~$v$`
- **Cuando frases matemáticas son representadas en palabras:**
	- `equals~$n$`
	- `less than~$\epsilon$`
	- `given~$X$`
	- `mod~2`
	- `modulo~$p^e$`
	- `for all large~$n$`
	- `is~15`
	- `is 15~times the height`
- **Cuando casos están siendo enumerados dentro de un párrafo:**
	- `(b)~Show that $f(x)$ is (1)~continuous; (2)~bounded`.

>[!nota]
> No hay reglas exactas para usar las ataduras, sobre todo se usa cuando hay peligro de que números o símbolos matemáticos sin contexto inicien una nueva línea. Como regla de oro es, si al omitir la parte anterior no se entiende bien lo que se quiere decir, entonces, probablemente debería ir una atadura por ahí.

Las ataduras permiten evitar rupturas en un espacio, sin embargo, si se quiere evitar una ruptura entre palabras o guiones, entonces se puede usar un `\hbox` ya que una vez que una caja es armada, esta es irrompible. Por ejemplo `\hbox{B-8}` hace que “B-8” siempre se muestre unido. Aunque por defecto $\TeX$ no suele separar líneas en guiones, por lo que no hay necesidad de insertar un `\hbox` en cada guion, salvo cuando un mal rompimiento se haya realizado.

Otro símbolo importante es la barra (/), en algunos lugares es deseable que la barra funcione como un punto de ruptura para las líneas. Esto se puede realizar mediante el comando `\slash`. Por ejemplo `input\slash output` permite que se pueda romper como “input/” y “output”.

También es posible indicar que se rompa la línea en un punto concreto, el comando `\break`. Sin embargo hay que tener cuidado con su uso, ya que puede introducir espacios muy grandes. Una forma de evitar esto es mediante `\hfil\break`, lo que hará que el texto se alinea a la derecha en esa línea y la siguiente línea no tenga sangría.

Finalmente, es posible hacer que los saltos de línea (`⏎`) funcionen como el final de un párrafo mediante el comando `\obeylines`. Este comando causará que todas los saltos de línea, a partir de cuando se coloque el comando, se vuelvan un comando `\par`, por lo que cada línea del código será una línea en el documento, a menos que la línea termine con `%` o sea tan grande que tenga que separarse en dos líneas. Por ejemplo, el siguiente código
```tex
{\obeylines\smallskip
Roses are red,
\quad Violets are blue;
Rhimes can be typeset
\quad With boxes and glue.
\smallskip}
```
se debería ver como:
>[!imagen]
>![[obeylines-ex.svg|600]]

---
## Algoritmo de ruptura de párrafos

Hablando en términos amplios, la forma en la que $\TeX$ rompe párrafos en líneas es mediante unas posiciones especiales llamadas “puntos de quiebre”, estos son agregados entre espacios o después de un guion, de tal manera que en cada uno de estos punto se calcula su “badness” y se asegura que no excedan el valor actual de `\tolerance`, si no existen ninguno de estos puntos es cuando sucede una caja “overfull”. Además del badness, se eligen estos puntos de quiebre basado en “deméritos”, que toman en cuenta el badness de cada una de las líneas además de la existencia de líneas consecutivas que terminan en guiones o líneas apretadas que ocurren después de líneas anchas.

### Lista horizontal

Ahora, en términos más concretos, antes de que las líneas se hayan roto, un párrafo dentro de $\TeX$ es en realidad una *lista horizontal*, es decir, una sucesión de elementos que $\TeX$ ha reunido mientras está en modo horizontal. En estas listas horizontales pueden haber los siguientes tipos de elementos:
- **Una caja:** un carácter, ligadura, regla, hbox o vbox.
- **Un quiebre discrecional**.
- **Un “whatsit”**.
- **Un material vertical** de `\mark`, `\vadjust` o `\insert`.
- **Un pegamento** o `\leaders`.
- **Un kern:** como un pegamento, pero que no se estira o encoge.
- **Un penalti:** que representa la indeseabilidad de una ruptura en ese punto.
- **math-on:** el inicio de una fórmula.
- **math-off:** el final de una fórmula.

Los últimos 5 elementos (pegamento, kern, penalti y elemento matemáticos) son llamados *descartables*, dado que pueden cambiar o desaparecer cuando se rompe una línea. Mientras que los primeros cuatro tipos son llamados *no descartables*, dado que siempre permanecen intactos.  Aunque algunos de estos elementos no han sido explicados, siempre es posible usar `\showlist` para ver exactamente lo que $\TeX$ está haciendo.

### Quiebres discrecionales

En primer lugar, un quiebre discrecional consiste en tres secuencias de caracteres, llamados textos de *pre-quiebre*, *pos-quiebre* y *no-quiebre*. La idea es que si un salto de página ocurre en ese punto, el texto de pre-quiebre aparecerá al final de la línea, mientras que el texto de pos-quiebre aparecerá al inicio de la siguiente línea. En el caso de que un salto de línea ocurra, el texto de no-quiebre será el que aparezca en su lugar. El usuario puede especificar un quiebre discrecional mediante la siguiente sintaxis
> [!sintaxis]
> `\discretionary{`⟨_pre-break text_⟩`}{`⟨_post-break text_⟩`}{`⟨_no break text_⟩`}`

donde los textos pueden ser caracteres, cajas y kerns.

Un ejemplo es la palabras “difficult”, si se quiere agregar un salto de línea en la ligatura “ffi” tenemos que el texto al final de la línea debe ser “dif-” mientras que al inicio de la siguiente será “ficult”. Esto se puede hacer en $\TeX$ mediante el siguiente comando:
```tex
di\discretionary{f-}{fi}{ffi}cult.
```
Afortunadamente $\TeX$ agrega automáticamente quiebres discrecionales mediante su algoritmo de guionado, que será explicado luego.

Otro ejemplo es el quiebre discrecional vacío `\discretionary{}{}{}`, el cual es agregado automáticamente después de un carácter `-`, lo que permite que las palabras puedan romperse después de este carácter (o sus respectivas ligaturas). En realidad cada fuente define su propio `\hyphenchar`, que por defecto es igual a `-`.

Otro quiebre discrecional importante es el guion discrecional, el cual puede ser agregado mediante el comando `\-`. Este es definido como:
```tex
\discretionary{-}{}{}
```
Este comando agrega manualmente un lugar en el que una palabra puede dividirse, en el caso que el algoritmo de guionado no pueda encontrar un buen punto de quiebre.

De hecho, cuando $\TeX$ separa una palabra con guiones, este simplemente agrega quiebres discrecionales en la lista horizontal. Por ejemplo la frase `discretionary hyphens` es transformado como
```tex
dis\-cre\-tionary hy\-phens
```
si el guionado es necesario. Sin embargo $\TeX$ no aplica el algoritmo de guionado a cualquier palabra que ya contenga una ruptura discrecional, por lo que se pueden usar (en casos de emergencia) para sobrescribir el guionado automático de $\TeX$.

Para ahorrar tiempo, $\TeX$ primero trata de romper los párrafos en líneas sin agregar guiones discrecionales. El primer paso tendrá éxito si una sucesión de puntos de ruptura es encontrado de tal manera que ninguno de las líneas resultantes tiene un badness que excede el valor actual de `\pretolerance`. Si este primer paso falla, el algoritmo de guionado es usado para separar cada palabra del párrafo al insertar guiones discrecionales en la lista horizontal, para luego hacer un segundo intento usando el valor de `\tolerance` en vez de `\pretolerance`.

Por defecto, $\TeX$ define `\pretolerance=100` y `\tolerance=200`. Sin embargo, si definimos `\pretolerance=10000` la primera prueba siempre tendrá éxito, por lo que no se intentará separar palabras por guiones. Por otro lado, si se definimos `\pretolerance=-1`, $\TeX$ omitirá el primer paso y tratará de separar palabras con guiones inmediatamente.

### Puntos de quiebre y penaltis

Los puntos de quiebre no solo pueden ocurrir entre palabras y guiones, de hecho son permitidos en los siguientes cinco casos:
1. En un pegamento, tal que sea inmediatamente precedido por un elemento no descartable y que no sea parte de una fórmula matemática (que no esté entre un math-on y un math-off). Una ruptura en un pegamento ocurre en el borde izquierdo del pegamento.
2. Un kern, tal que sea inmediatamente seguido de un pegamento y que no sea parte de una fórmula matemática.
3. En un math-off que sea inmediatamente seguido por pegamento.
4. En un penalti, el cual podría haber sido agregado automáticamente en una fórmula.
5. En un quiebre discrecional.

Cada punto de quiebre potencial tiene asociado un “penalti” el cual representa el “costo estético” de hacer una ruptura en ese lugar. En los casos 1, 2, y 3 el penalti es cero. En el caso 4 un penalti explícito ha sido especificado. En el caso 5 el penalti es el valor actual de `\hyphenpenalty`, si el texto de pre-quiebre es no vacío, o el valor actual de `\exhyphenpenalty` si el texto de pre-quiebre es vacío. Por defecto tenemos que `\hyphenpenalty=50` y `\exhyphenpenalty=50`.

En el caso 4, se puede colocar penaltis explícitos mediante el comando `\penalty`⟨_number_⟩. Por ejemplo si se coloca `\penalty 100` en algún punto del párrafo, esa posición será un punto de quiebre legítimo, sin embargo un penalti de 100 será cargado.

También es posible agregar `\penalty-100`, lo que indicará a $\TeX$ que ese es un buen lugar para hacer un salto de línea, ya que un penalti negativo es en realidad una “bonus” y una línea que termina con bonus podría incluso tener “méritos” (deméritos negativos).

Cualquier penalti que sea 10000 o mayor será considerado tan grande que $\TeX$ nunca hará un salto de línea ahí. En el otro extremo, cualquier penalti que sea -1000 o menor, será considerado tan pequeño que $\TeX$ siempre hará un salto de línea ahí. Por ejemplo, el comando `\nobreak` es simplemente una abreviatura para `\penalty10000`, lo que prohíbe hacer un salto de línea. Las ataduras (`~`) son equivalentes a `\nobreak\␣`, notar como no es posible un quiebre entre el `\nobreak` y el espacio ya que un pegamento nunca es un punto de quiebre cuando es precedido de un elemento descartable como un penalti.

Cuando un salto de línea sucede, $\TeX$ remueve todos los elementos descartables que siguen al quiebre, hasta que llega a algo no descartable o hasta que llega a otro punto de quiebre elegido. Por ejemplo, una sucesión de pegamentos y penaltis serán removidos como una unidad, si no hay cajas que intervengan, a menos que la sucesión de puntos de quiebre óptimo incluya uno o más de estos penaltis.

Los math-on y math-off,  actúan esencialmente como kerns que agregan el espacio especificado por `\mathsurround`. Tal espacio desaparecerá en un salto de línea si una fórmula está al final o al inicio de una línea, debido a la manera en la que las reglas han sido formuladas anteriormente.

### Badness

El badness de una línea es un entero que es aproximadamente 100 veces el cubo del ratio por el cual el pegamento dentro de la línea debe ser estirado o encogido para hacer un hbox del tamaño requerido.

Por ejemplo, si una línea tiene un encogimiento total de 10 pt y el pegamento será comprimido en un total de 9 pt, entonces el badness calculado será de 73 ya que $100(9/10)^3 = 72.9$. De manera similar, una línea que debe ser estirada el doble tendrá un badness de 800. Sin embargo, si el badness obtenido por este método es más grande que 10000, el valor de 10000 será usado.

Más concretamente, si hay estiramiento o encogimiento infinito, el badness será 0, de otra forma, el badness será aproximadamente $\min(100r^3, 10000)$ donde $r$ es el ratio de colocación de pegamento. Además, las cajas overfull son consideradas infinitamente malas, por lo que será evitadas siempre que sea posible.

> [!Nota]
> Si una línea no tiene estirado, entonces, por el calculo del ratio de colocación de pegamento tendremos que $r=0$, sin embargo, si el ancho del contenido no es suficiente para llenar la caja, entonces tendremos una caja underfull con badness 10000.

Una línea cuyo badness sea 13 o superior tiene un ratio de colocación de pegamento mayor al 50%. Además, si el badness es 13 o superior, decimos que una línea es *apretada* si el pegamento se tiene que encoger, *holgada* si el pegamento se tiene que estirar y *muy holgada* si tiene que estirarse tanto que el badness sea de 100 o más. Sin embargo, si el badness es 12 o menor, decimos que la línea es *decente*. 

Ordenando dependiendo de su clasificación, tenemos que una línea puede ser: apretada, decente, holgada o muy holgada. De este modo, decimos que dos líneas son *visualmente incompatibles* si su clasificación no es adyacente. Por ejemplo, si una línea ajustada está antes de una holgada o muy holgada, o si una línea decente está antes de una muy holgada.

### Deméritos

$\TeX$ califica cada sucesión potencial de puntos de quiebre al sumar el total de *deméritos* evaluados por cada línea individual. El objetivo es elegir puntos de quiebre que den la menor cantidad de deméritos.

Para ello, supongamos que una línea tiene badness $b$ y que un penalti $p$ es asociado con el punto de quiebre al final de esa línea. En primer lugar, como se estableció anteriormente $\TeX$ ni siquiera considerará una línea tal que $p \geq 10000$ o si $b$ excede el valor actual de `\tolerance` o `\pretolerance` actual. De otra otra forma, los deméritos de la línea son definidos por la fórmula
$$ d = \begin{cases}
		(l+b)^2 + p^2, & \text{si}\ 0 \leq p < 10000;\\
		(l+b)^2 - p^2, & \text{si}\ -10000 \leq p < 0;\\
		(l+b)^2 & \text{si}\ p \leq -10000.
	\end{cases} $$
En la fórmula anterior, $l$ es el valor actual de `\linepenalty`, un parámetro que puede ser incrementado si se desea que $\TeX$ se esfuerce más en mantener los párrafos al mínimo número de líneas. Por defecto tenemos que  `\linepenalty=10`.

Por ejemplo, una línea con badness 20 que termina en un pegamento, tendrá $(10+20)^2 = 900$ deméritos, dado que $l=10$ y no hay penalti por romper en un pegamento. De este modo, minimizar los deméritos es equivalente a minimizar  la suma de los cuadrados de los badness y penaltis. Esto usualmente también significa que el máximo badness de cada línea es también minimizado.

Deméritos adicionales son agregados basados en pares de líneas adyacentes. Si dos líneas consecutivas son visualmente incompatibles, entones el valor actual de `\adjdemerits` es agregado a $d$. Si dos líneas consecutivas terminan con quiebres discrecionales, entonces `\doublehyphendemerits` es agregado. Y si la penúltima línea del párrafo termina con un quiebre discrecional, entonces `\finalhyphendemerits` es agregado. Por defecto se define:
```tex
\adjdemerits=10000
\doublehyphendemerits=10000
\finalhyphendemerits=5000
```

>[!nota]
> Los deméritos están en unidades de “badness al cuadrado”, por ello los parámetros basado en deméritos tienen que ser números bastante grandes, a diferencia de las tolerancias y penaltis que su unidad es la misma que el badness.

---
Es posible ver el proceso interno de como $\TeX$ rompe los párrafos en líneas mediante el comando `\tracingparagraphs=1`. Por ejemplo, el siguiente código
```tex
\hsize=2.5in \tolerance=1000 \tracingparagraphs=1
Mr.~Drofnats---or ``R. J.,'' as
he preferred to be called---%
was happiest when he was at work
typesetting beautiful documents.
```
dará como resultado el siguiente texto en el `.log`:
```tex
[]\tenrm Mr. Drofnats---or ``R. J.,'' as he pre-
@\discretionary via @@0 b=0 p=50 d=2600
@@1: line 1.2- t=2600 -> @@0
ferred to be called---was hap-pi-est when
@ via @@1 b=131 p=0 d=29881
@@2: line 2.0 t=32481 -> @@1
he
@ via @@1 b=25 p=0 d=1225
@@3: line 2.3 t=3825 -> @@1
was at work type-set-ting beau-ti-ful doc-
@\discretionary via @@2 b=1 p=50 d=12621
@\discretionary via @@3 b=291 p=50 d=103101
@@4: line 3.2- t=45102 -> @@2
u-
@\discretionary via @@3 b=44 p=50 d=15416
@@5: line 3.1- t=19241 -> @@3
ments.
@\par via @@4 b=0 p=-10000 d=5100
@\par via @@5 b=0 p=-10000 d=5100
@@6: line 4.2- t=24341 -> @@5
```

En el texto anterior, tenemos que las líneas que inician con `@@` indican los *puntos de quiebre factibles*, es decir, puntos de quiebre que pueden ser usados sin que el badness exceda la tolerancia. Los puntos factibles se enumeran consecutivamente iniciando con `@@1`. El inicio del párrafo es considerado también un punto factible y es enumerado como `@@0`. Mientras que las líneas que inician con un solo `@` indican las formas posibles de usar esos puntos de quiebre. $\TeX$ solo elegirá el mejor candidato cuando haya una opción.

Las líneas que no inician con `@`, indican hasta que parte del párrafo $\TeX$ ha leído. Por ejemplo, tenemos que `@@2: line 2.0 t=32481 -> @@1` está después de `...hap-pi-est when` y antes de `he`. De esta forma, el párrafo anterior estaría dividido de la siguiente manera:
```tex
(@@0) Mr.~Drofnats---or ``R. J.,'' as he pre-
(@@1) ferred to be called---was happiest when
(@@2) he
(@@3) was at work typesetting beautiful doc-
(@@4) u-
(@@5) ments.
(@@6)
```

La notación `line 2.0`, en el ejemplo anterior, significa que el quiebre factible terminaría como el final de la línea 2 y que será muy holgada, ya que el número después del punto, indica la clasificación de la línea, teniendo `.0` para una línea muy holgada, `.1` para una línea holgada, `.2` para una línea decente y `.3` para una línea apretada. También, un guion acompañará el número de línea si la línea termina con un quiebre discrecional o si es el final del párrafo, por ejemplo `line 1.2-`.

Además, la notación `t=32481`, en el ejemplo anterior, indica el total de deméritos del inicio del párrafo hasta el punto `@@2`. Mientras tanto, la notación `-> @@1` indica que la mejor manera de usar el punto `@@2` es mediante `@@1`.

Notemos que la línea `@ via @@1 b=131 p=0 d=29881` indica que el badness de `@@1` a `@@2` es de $b = 131$ y tiene penalti $p = 0$. Además, el punto de quiebre `@@1` da como resultado una línea decente, mientras que `@@2` da una línea muy holgada. De este modo, el total de deméritos de `@@1` a `@@2` es
$$
\begin{align}
d &= (l+b)^2 + p^2 + \texttt{\\adjdemerits} \\
	&= (10+131)^2 + 0^2 + 10000 \\
	&= 29881.
\end{align}
$$
Similarmente el punto `@@3` presenta una una forma distinta de construir la segunda línea desde `@@1` con deméritos $d=1225$. El siguiente punto factible es `@@4` que puede iniciar en `@@2`, con deméritos $d = 12621$, o iniciar en `@@3`, con deméritos $d=103101$. Procediendo análogamente con todos los puntos de quiebre, es posible crear el siguiente grafo acíclico dirigido:

>[!gráfico]
> ```mermaid
> flowchart TD
> 	BP0[@@0]
>     BP1[@@1]
>     BP2[@@2]
>     BP3[@@3]
>     BP4[@@4]
>     BP5[@@5]
>     BP6[@@6]
> 
>     BP0 -->|d = 2600| BP1 -->|d = 29881| BP2 -->|d = 12621| BP4 -->|d = 5100| BP6
>     BP1 -->|d = 1225| BP3 -->|d = 103101| BP4
>     BP3 -->|d = 15416| BP5 -->|d = 5100| BP6
> 
>     classDef default font-family:'Cascadia Code NF',monospace;
> ```

De este modo, $\TeX$ encuentra la mejor secuencia de puntos de quiebre al calcular el “camino más corto” de `@@0` a cada uno de los puntos de quiebre. En el ejemplo anterior, el camino más corto de `@@0` a `@@6` es a través de `@@5`, `@@3` y `@@1`.

---
Si no es posible elegir un punto de quiebre factible, $\TeX$ elegirá un punto de quiebre no factible y lo representará en el desglose de `\tracingparagraphs` como `b=*`. En estos casos, no se calcula sus deméritos, por lo que no se toma en consideración a la hora de elegir la sucesión de puntos de quiebre óptimo.

Por ejemplo, en el texto anterior, si cambiamos el `\hsize` a 1.75 in, tenemos el siguiente texto en el `.log`:
```tex
[]\tenrm Mr. Drofnats---or ``R. J.,''
@ via @@0 b=* p=0 d=*
@@1: line 1.3 t=0 -> @@0
as he pre-ferred to be called---
@\discretionary via @@1 b=10 p=50 d=2900
@@2: line 2.2- t=2900 -> @@1
was hap-pi-est when he was
@ via @@2 b=562 p=0 d=337184
@@3: line 3.0 t=340084 -> @@2
at
@ via @@2 b=0 p=0 d=100
@@4: line 3.2 t=3000 -> @@2
work type-set-ting beau-ti-
@\discretionary via @@3 b=267 p=50 d=79229
@@5: line 4.0- t=419313 -> @@3
ful
@ via @@3 b=4 p=0 d=10196
@@6: line 4.2 t=350280 -> @@3
doc-u-ments.
@\par via @@5 b=0 p=-10000 d=15100
@\par via @@6 b=0 p=-10000 d=100
@@7: line 5.2- t=350380 -> @@6
```

Podemos ver como `@@1` es un punto no factible y que su total de deméritos es `t=0`. Además, obtendremos una alerta de `Overful \hbox` en esa respectiva línea.

> [!nota]
> Cuando aparece `d=*`, en general, significa que no aporta nada al total de deméritos. Esto también puede suceder cuando se agrega un salto de línea manual o en el último punto factible (final de párrafo) aunque no sea un overfull hbox.

---
La última línea de un párrafo es especial, ya que esta se alinea a la derecha. Para lograr esto, antes de que $\TeX$ empiece a elegir puntos de ruptura, se hace dos cosas importantes:
1. Si el elemento final de la lista horizontal actual es un pegamento, entonces es descartado.
2. Tres elementos más son agregados al final de la lista horizontal actual:
	1. Un `\penalty10000`, para que no haya un salto de línea.
	2. Un `\hskip\parfillskip`, el cual agrega el “pegamento de finalizado”.
	3. Un `\penalty-10000`, para forzar el salto de línea final.

Por defecto `\parfillskip=0pt plus1fill`, por lo que la línea final de cada párrafo estará alineada a la derecha.

---
$\TeX$ tiene dos parámetros llamados `\leftskip` y `\rightskip` los cuales especifican pegamentos que serán agregados a la izquierda y derecha de cada línea en un párrafo. Este pegamento es tomado en cuenta cuando el badness y los deméritos son calculados.

Por defecto, estos valores están inicializados en 0pt. Sin embargo, el comando `\narrower` incrementa ambos valores en la cantidad actual de `\parindent`. Por ejemplo, consideremos el siguiente código
```tex
This paragraph comes before the narrower paragraph to see a normal paragraph.

{\narrower\smallskip\noindent
This paragraph will have narrower lines
than the surrounding paragraphs do, because it
uses the ``narrower'' feature of plain \TeX.
The former margins will be restored after
this group ends.\smallskip}

This paragraph comes after the narrower paragraph to see a normal paragraph.
```
se debería ver como:
>[!imagen]
>![[narrower-ex.svg|600]]

Es importante terminar el párrafo antes de terminar el grupo ya que en caso contrario el efecto de `\narrower` desaparecerá antes de que $\TeX$ empiece a elegir puntos de quiebre. En el ejemplo anterior el segundo `\smallskip` sirve para terminar el párrafo.

Hay que tener cuidado con colocar pegamento infinito en `\leftskip` o `\rightskip`, ya que esto hará que todas las líneas, incluso las más cortas, tengan badness 0, lo que complica el algoritmo para separar un párrafo en líneas.

---
$\TeX$ solo revisa los parámetros que afectan los saltos de líneas cuando está empezando a calcular los saltos de línea. De este modo, si se modifica alguno de estos valores (como \hyphenpenalty, \rightskip, \hsize, etc.) dos o más veces en un mismo párrafo, solo se tomará en cuenta el valor cuando el párrafo termine, por lo que el otros valores no será tomados en cuenta.

La única excepción a esta regla es la sangría, este usará el valor de `\parindent` en el momento que la sangría sea agregada a la lista lista horizontal en vez de su valor al finalizar el párrafo. 

Algo similar pasa con algunos penaltis que se agregan en modo matemático, ya que el valor que se usará será el que esté al final de esa fórmula matemática en concreto.

---
