# Pegamento
Además de las cajas, $\TeX$ maneja con otro elemento llamado “pegamento”. Este es el espacio que une a las cajas. La principal diferencia con respecto a las cajas es que el pegamento se puede estirar y encoger.

El pegamento tiene tres valores importantes: *espacio*, *encogimiento* y *estiramiento*. Por ejemplo consideremos la siguiente imagen:
>[!imagen]
>![[glue-ex-1.svg|540]]

Se puede ver cuatro cajas unido por tres pegamentos. Dado que la longitud natural (anchura de las cajas más el espacio de los pegamentos) es igual a 52 (anchura de la caja), entonces los pegamentos permanecen inalterados. 

Sin embargo si queremos que la caja tenga una anchura de 58, los pegamentos se tendrán que estirar de la siguiente manera:
1. Se calcula la diferencia de la anchura de la caja y  la longitud natural: $d = 58-52=6$.
2. Se calcula la suma de los valores de estiramiento: $s = 3+6+0=9$
3. Se multiplica el estiramiento de cada pegamento por el cociente $d/s=6/9$.
4. Se suma este factor de estiramiento al espacio de cada pegamento.

De esta forma, el ejemplo anterior quedaría como:
>[!imagen]
>![[glue-ex-2.svg|600]]

Si la caja tiene una anchura menor que la longitud natural, el proceso se hará de la misma forma, a excepción de que se usaran los valores de encogimiento y al final se restará al espacio de cada pegamento.

Sin embargo, una diferencia entre el estiramiento y el encogimiento es que los pegamentos se pueden estirar infinitamente (siempre que el valor de estiramiento sea positivo), mientras que solo se pueden encoger como máximo la cantidad preestablecida. Por lo que la mínima anchura del ejemplo anterior es de 49 unidades. 

El proceso en el que se determina la anchura del pegamento se llama *colocando el pegamento*. Una vez que se haya colocado, este se volverá rígido. La mejor manera de establecer los valores de un pegamento es el siguiente:
- El espacio de un pegamento debe ser la longitud tal que se vea lo mejor posible.
- El estiramiento debe ser lo máximo que se le puede agregar al pegamento antes de que se vea mal.
- El encogimiento debe ser la máxima cantidad que se le puede quitar antes de que se vea mal.

$\TeX$ tiene varios pegamentos definidos. Estos son `\smallskip`, `\medskip` y `\bigskip`. El primero es un espacio vertical que agrega 3 pt y se puede estirar o encoger 1 pt. Un `\medskip` son dos veces un `\smallskip` y un `\bigskip` son dos veces un `\medskip`. 

---
Los pegamentos horizontales y verticales se incluyen mediante los comando `\hskip`⟨_glue_⟩ y `vskip`⟨_glue_⟩, respectivamente, donde ⟨_glue_⟩ es una unidad definida como:
> [!sintaxis]
> ⟨_dimen_⟩ `plus` ⟨_dimen_⟩ `minus`⟨_dimen_⟩

Los valores `plus` ⟨_dimen_⟩ y  `minus`⟨_dimen_⟩ son parámetros opcionales, el primer ⟨_dimen_⟩ siempre debe agregarse (aunque el valor sea cero) y en los otros dos se asume que ⟨_dimen_⟩ es cero si no se está presente. Por ejemplo `\medskip` está definido como
```tex
\vskip6pt plus 2pt minus2pt
```
Otros ejemplo es el comando `\enskip` que está definido como `\hskip.5em\relax`, es decir, un espacio horizon tal de exactamente medio em en la fuente actual, no se estira y no se encoge.

>[!nota]
>El `\relax` después de la definición de `\enskip` es para que deje de leer cualquier texto posterior, eso evita que una (posible) palabra `plus` o `minus` sea considerada parte de la unidad de pegamento.

---
Es posible hacer que un pegamento se encoja y estire infinitamente. El resultado es que siempre se quiera estirar o encoger, este lo hará en el pegamento (o pegamentos) que tengan sus valores infinitos. Esto puede ser útil para centrar líneas o que se alineen a la izquierda.

Un ejemplo de esto son los comandos `\vfil` y `\vfill`. Los dos indican que se debe colocar un pegamento de espacio 0 pt pero infinitamente estirable. Esto es útil cuando se quiere colocar un texto al final de la página.

La diferencia entre `\vfil` y `\vfill`, es que el tercero es infinitamente más grande que el segundo y este es infinitamente más grande que el tercero. Es decir, si un `\vfil` y un `\vfill` han sido colocados, el pegamento que siempre se estirará será el `\vfill`, mientras que el `\vfil` no lo hará.

También existen sus variantes horizontales, estos son `\hfil` y `\hfill`. Estos comandos pueden ser útiles para centrar, o alinear a la derecha o izquierda un texto.

Otro ejemplo son los comandos `\hss` y `\vss` que indican pegamentos, horizontal y vertical, respectivamente, cuyo espacio es 0 pt, pero que se pueden estirar o encoger infinitamente.

Finalmente, tenemos las primitivas `\hfilneg` y `\vfilneg` que cancelan el estiramiento de `\hfil` y `\vfil`, respectivamente.

Algunos ejemplos de estos comandos se pueden ver en el siguiente código, estos utilizan el comando `\line` que únicamente crea una caja cuya anchura es la misma que `\hsize`:
```tex
\line{This text will be flush left.\hfil}
\line{\hfil This text will be flush right.}
\line{\hfil This text will be centered.\hfil}
\line{Some text flush left\hfil and some flush right.} \line{Alpha\hfil centered between Alpha and Omega\hfil Omega}
\line{Five\hfil words\hfil equally\hfil spaced\hfil out.}
```
>[!imagen]
>![[hfil-ex.svg|600]]

Es posible crear pegamentos propios que se estiren o encojan infinitamente. Para ello se utilizan unas unidades especiales, que puede ser usada en`plus` ⟨_dimen_⟩ o `minus`⟨_dimen_⟩, llamadas `fil`, `fill` y `filll`. Por ejemplo, los comandos `\vfil`, `\vfill`, `\vss` y `\vfilneg` son respectivamente equivalentes a los siguientes pegamentos:
```tex
\vskip 0pt plus 1fil
\vskip 0pt plus 1fill
\vskip 0pt plus 1fil minus 1fil
\vskip 0pt plus -1fil
```

Como regla de oro para utilizar las unidades `fil`, `fill` y `filll` es que las unidades `fil` deben ser siempre la unidad a usar, si aun así no sale el resultado requerido se puede usar la unidad `fill`, solo en casos muy extremos hay que usar la unidad `filll`.

Asimismo, se puede usar múltiplos fraccionales de las unidades `fil`, `fill` y `filll`, como `3.25fil` siempre y cuando este número sea menor que 16384. Más concretamente, $\TeX$ hace sus cálculos de estas unidades en múltiplo enteros de $2^{-16}$, por lo que `0.000007filll` es indistinguible de `0pt`, sin embargo `0.00001filll` es infinitamente mayor que `16383.99999fill`.

---
$\TeX$ tiene una regla especial con respecto al final de las oraciones: este agrega un poco más de espacio, así como incrementar su estiramiento y reducir su encogimiento, después de un signo de puntuación. Esto con el fin de mejorar la lectura.

Por ejemplo, considera la frase: ``` ``Oh oh!'' cried Baby Sally. Dick and Jane laughed. ```, este se renderizará, en su anchura natural y siendo estirado 5 pt, 10 pt, 15 pt y 20 pt, respectivamente, como: 
>[!imagen]
>![[colon-strech.svg|600]]

Específicamente, el pegamento después de una coma se estira 1.5 veces más que un espacio normal (el que hay entre palabras) y se encoge 0.8 veces el espacio normal, mientras que el espacio después de punto, signo de exclamación o interrogación,  se estira 3 veces más que un espacio normal y se encoge 0.3 veces el espacio normal. No hay pegamento entre letras adyacentes, por lo que las palabras individuales permanecen inalteradas.

Sin embargo, esta regla hace que se necesiten algunas consideraciones a la hora de escribir. En primer lugar, para los puntos suspensivos se debe usar `\ldots` en el entorno matemático (ya que en este modo tiene sus propias reglas de espaciado). De este modo `A $\ldots$ suspense` se debería ver como “A $\ldots$ suspense”.

Otro caso especial son con las abreviaciones. Como regla general, es mejor usar el carácter `~` para las abreviaturas, como en `Fig.~5` ya que agrega un espacio irrompible (no se puede dividir la línea en ese espacio). Sin embargo, si se requiere abreviaciones y espacios rompibles (como en la bibliografía) siempre se puede colocar el comando `\␣` después del punto, como en:
```tex
Proc.\ Amer.\ Math.\ Soc.
```

De igual forma, $\TeX$ no considera que una oración termina (por ende no agrega el espacio extra) cuando el signo de puntuación es precedido de una letra mayúscula. Esto puede ser arreglado colocando el comando `\null` antes del signo de puntuación, como en `Did I\null?`.

Finalmente, es posible eliminar este espacio extra al final de las oraciones mediante el comando `\frencspacing`, este comando hace que todos los espacios tengan la misma longitud. De igual forma, se puede usar el comando `\nonfrenchspacing` para  agregar nuevamente el espacio extra o se puede restringir el comando `\frencspacing` usando un grupo .

---
Es posible ver el pegamento que $\TeX$ pone entre las palabras usando `\showbox`. Por ejemplo la caja de “Baby Sally”, asumiendo `\nonfrenchspacing` se ve como
```tex
.\tenrm \ (ligature ``)
.\tenrm O
.\tenrm h
.\tenrm ,
.\glue 3.33333 plus 2.08331 minus 0.88889
.\tenrm o
.\tenrm h
.\tenrm !
.\tenrm " (ligature '')
.\glue 4.44444 plus 4.99997 minus 0.37036
.\tenrm c
.\tenrm r
.\tenrm i
.\tenrm e
.\tenrm d
.\glue 3.33333 plus 1.66666 minus 1.11111
.\tenrm B
.\tenrm a
.\tenrm b
.\kern-0.27779
.\tenrm y
.\glue 3.33333 plus 1.66666 minus 1.11111
.\tenrm S 
.\tenrm a 
.\tenrm l 
.\tenrm l 
.\tenrm y 
.\kern-0.83334 
.\tenrm .
.\glue 4.44444 plus 4.99997 minus 0.37036
```

Se puede ver como un espacio entre palabras normal es de 3.33333 pt con estiramiento de 2.08331 o y encogimiento de 0.88889 pt. Además se puede ver como cambia después de un signo de puntuación, como convierte las ligaturas y agrega algunos `\kern` para mejorar el espaciado.

>[!nota]
>Un `\kern` es como un pegamento, solo que no se estira ni encoge. Además las palabras nunca se podrán romper en un kern, salvo que vaya seguido inmediatamente de un pegamento.

---
$\TeX$ tiene unas reglas muy precisas de cuanto espacio agregar, esto es lo que permite que haya más espacio después de un signo de puntuación y que este cambia dependiendo del carácter previo.

Cuando $\TeX$ está procesando una lista horizontal de cajas y pegamentos, este revisa un valor llamado  “factor de espacio”. Normalmente este valor es 1000, lo que significa que el espacio entre palabras no es modificado.

Ahora, si el factor de espacio $f$ difiere de 1000, el pegamento que va entre las palabras es calculado de la siguiente manera:
- Cada fuente especifica el tamaño de su espacio, su estiramiento normal y su encogimiento normal, así como el espacio extra.
- Se agregará el espacio extra si $f \geq 2000$.
- El componente de estirado es multiplicado por $f/1000$.
- El componente de encogido es multiplicado por $1000/f$.

Existen dos parámetros, `\spaceskip` y `\xspaceskip` que permite sobrescribir el espaciado normal de una fuente actual. Si $f \geq 2000$ y `\xspaceskip` es distinto de cero, el pegamento `\xspaceskip` será usado como el espacio entre las palabras. De otra forma, si `\spaceskip` es distinto de cero, el pegamento `\spaceskip` será usado con sus componentes de estirado y encogido multiplicados por $f/1000$ y $1000/f$, respectivamente. Por ejemplo, el comando `\raggedright` usa estos comandos para suprimir el estirado y encogido de los espacios entre palabras.

El factor de espacio $f$ es 1000 al inicio de una lista horizontal y se establece a 1000 justo después de que una caja no carácter o una fórmula matemática sea puesta en la lista horizontal actual. También es posible usar el comando `\spacefactor=`⟨_number_⟩ para asignar un valor específico al factor de espacio. 

Cada carácter tiene su propio código de factor de espacio $g$ y cuando aparece en la lista cambia el factor de espacio de la siguiente forma:
- Si $g=0$, entonces el factor de espacio anterior $f$ no es cambiado.
- Si $f < 1000 <g$ entonces el factor de espacio se vuelve 1000.
- En cualquier otro caso, $g$ se vuelve el nuevo factor de espacio.
La segunda regla indica que el factor de espacio no salta de un valor menor a 1000 a uno mayor a 1000, esto es para asegurar que el factor de espacio no cambie bruscamente.

> [!nota]
> La cantidad máxima de factor de espacio permitido es de 32767

Cuando `INITEX` crea una nuevo formato de $\TeX$, todos los caracteres tienen un factor de 1000, a excepción de las letras mayúsculas, `A` hasta `Z`, que tienen código 999. Plain $\TeX$ redefine algunos de los códigos de factor de espacio usando la primitiva `\sfcode` (que es similar a `\catcode`), por ejemplo, las instrucciones
```tex
\sfcode`)=0
\sfcode`.=3000
```
hacen a el paréntesis derecho “transparente” al factor de espacio, mientras que triplica el estiramiento de los espacios después de un punto. El comando `\frenchspacing` simplemente resetea el factor de espacio del punto a 1000.

Cuando una ligatura se forma, o cuando se inserta un carácter especial usando `\char`, el factor de espacio des calculado de los caracteres individuales que generan esa ligadura. Por ejemplo, plain $\TeX$ asigna a `'` un factor de espacio de 0, por lo que la ligatura `''` también tendrá factor de espacio de 0 a pesar de que el carácter en la posición '042 (que es donde el carácter de la ligatura se sitúa) no tenga factor de espaciado 0.

---
El algoritmo que usa $\TeX$ para determinar el espacio que un pegamento tomará en una caja horizontal es el siguiente: 
- La anchura natural $x$ de la caja es determinada al agregar todas las anchuras de las cajas, kerns y la anchura natural de todos los pegamentos dentro de la caja.
- Se calcula el total de estiramiento $y_0 +y_1\,\text{fil} +y_2\,\text{fill} +y_3\,\text{filll}$ y encogimiento $z_0 +z_1\,\text{fil} +z_2\,\text{fill} + z_3\,\text{filll}$ de los pegamentos dentro de la caja.
- Se compara la anchura natural con el ancho deseado $w$. Si $x = w$ todos los pegamentos obtendrán su anchura natural, sin embargo en caso contrario se calcula la “proporción de pegamento” $r$ y el “orden del pegamento” $i$ de la siguiente manera:
	1. Si $x<w$ se tratará de estirar los pegamentos. El orden $i$ será el mayor tal que $y_i \neq 0$ y la proporción será $r = (w-x)/y_i$. En el caso que todos los $y_i=0$ entonces $r=i=0$.
	2. Si $x>w$ se tratará de encoger los pegamentos. El orden $i$ será el mayor tal que $z_i \neq 0$ y la proporción será $r = (x-w)/z_i$. Sin embargo $r = 1$ en el caso que $i=0$ y $x-w>z_0$ ya que el encogido máximo no puede ser excedido. 
- Cada pegamento es modificado de la siguiente forma: Supongamos que un pegamento específico tiene anchura natural $u$, estiramiento $y$ y encogido $z$, donde $y$ es un $j$-ésimo orden de infinito y $z$ es un $k$-ésimo orden de infinito.
	- Si $x<w$ (estirado) el pegamento tendrá nueva anchura $u+ry$ si $j=i$, mientras que mantendrá su anchura $u$ si $j \neq i$.
	- Si $x>w$ (encogido) el pegamento tendrá nueva anchura $u-rz$ si $k=i$, mientras que mantendrá su anchura $u$ si $j \neq i$.

>[!nota]
>El estirado y encogido solo ocurre cuando el pegamento tiene el mayor orden de infinito que no se cancela. De este modo, un pegamento de estirado finito no se estirará si en la misma caja hay un pegamento de estirado infinito.

---
$\TeX$ puede construir una caja de anchura dada si se usa el comando `\hbox to`⟨_dimen_⟩`{`⟨_content of box_⟩`}` donde ⟨_dimen_⟩ será la anchura exacta de la nueva caja. Por ejemplo el comando `\line` es una abreviación de ```
```tex
\hbox to\hsize
```
También es posible indicar una cantidad exacta de estirado (o encogido) para una caja usando el comando `\hbox spread`⟨_dimen_⟩`{`⟨_content of box_⟩`}` donde ⟨_dimen_⟩ será la cantidad que se le agregará (o quitará) a la anchura natural del contenido. Por ejemplo, una de las cajas anteriores fue escrita usando:
```tex
\hbox spread 5pt{``Oh, oh!'' ... laughed.}
```
Finalmente, si quieres una caja cuya anchura sea la natural, basta con usar `\hbox{`⟨_content of box_⟩`}`.

En todos los casos, la línea base de una caja horizontal construida es la línea base en común de su contenido (en el caso de que no hayan sido alzados o bajados). La altura y profundidad son la mayor distancia por arriba y por abajo (con respecto a la línea base) que alcanzan la cajas que lo componen. Por ende, el resultado de una `\hbox` nunca tiene altura o profundidad negativa, sin embargo puede tener anchura negativa.

---
$\TeX$ coloca las cajas verticales de manera similar a las horizontales, sin embargo este difiere en que la colocación usualmente es de tal manera que las líneas base siempre están a una distancia fija una de otras.

Esto se logra mediante tres primitivas llamadas `\baselineskip`, `\linesip` y `\lineskiplimit`. La sintaxis es la siguiente:
>[!sintaxis]
>`\baselineskip=`⟨_glue_⟩
>`\lineskip=`⟨_glue_⟩
>`\lineskiplimit=`⟨_dimen_⟩

Cada vez que una caja es agregada a la lista vertical, $\TeX$ agrega un “pegamento de interlineado” cuya función es hacer que la distancia entre las líneas base de la nueva caja y la anterior sea exactamente igual a `\baselineskip`.  Sin embargo, si el pegamento de interlineado calculado por esta regla causa que la esquina superior de la nueva caja sea más cercana que `\lineskiplimit` a la esquina inferior de la caja anterior, entonces `\lineskip` es usado como pegamento de interlineado. En otras palabras, la distancia entre líneas base adyacentes será `\baselineskip` a menos que eso cause que las cajas estén muy cercanas, en ese caso se usará `\lineskip` para separar cajas adyacentes.

Las reglas para el pegamento de interlineado son llevadas a cabo sin tener en cuenta otros tipos de pegamento que podrían estar presentes. Es decir, todo el espacio vertical dado por una aparición explícita de `\vskip` y `\kern` actual independiente al pegamento de interlineado. De este modo, un `\smallskip` entre dos líneas siempre separa sus líneas base más que de lo que usualmente estarían, por una cantidad de un `\smallskip`, este comando no afecta la decisión acerca de cuando el pegamento `\lineskip` será usado entre esas líneas.

Por ejemplo, supongamos que `\baselineskip=12pt plus 2pt`, `\lineskip=3pt minus 1pt` y `\lineskiplimit=2pt`. Además, supongamos que una caja cuya profundidad es de 3 pt fue la más recientemente agregada a la lista vertical actual. Estamos a punto de agregar una nueva caja cuya altura es $h$. Si $h = 5\,\text{pt}$, el pegamento de interlineado será de `4pt plus 2pt`, ya que eso hará que las líneas base estén alejadas `12pt plus 2pt`  cuando agreguemos $h$ y la profundidad anterior al pegamento de interlineado.
> [!imagen]
> ![[vbox-ex-1.svg|600]]

Sin embargo, si $h=8\,\text{pt}$, el pegamento de interlineado será 3 pt minus 1 pt, dado que `\lineskip` será usado como pegamento de interlineado. Ya que en caso contrario, el pegamento de interlineado, ignorando el estirado y encogido, sería de $1\,\text{pt}= 12\,\text{pt}-3\,\text{pt}-8\,\text{pt}$, que es menor que `\lineskiplimit`.
> [!imagen]
> ![[vbox-ex-2.svg|600]]

>[!nota]
>Como regla genera, si se tiene un documento de muchas páginas lo mejor es  definir `\baselineskip` para que no se estire ni encoja. Sin embargo, si es un documento de una sola página, darle estirado a `\baselineskip` puede ayudar a $\TeX$ a llenar el contenido en la página.

---
