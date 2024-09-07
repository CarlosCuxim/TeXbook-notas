# Fuentes tipográficas

$\TeX$ cuenta con multiples tipos de fuente y comandos para cambiar estos. Algunas de estas fuentes son las siguientes:
- `\rm` cambia estilo “$\rm roman$”
- `\sl` cambia al estilo “slanted”
- `\it` cambia al estilo “$\it italic$”
- `\tt` cambia al estilo $\tt monospace$
- `\bf` cambia al estilo $\bf bold$

Es posible cambiar el estilo en una parte de texto usando las llaves `{` y `}`, por ejemplo
```tex
to be {\bf bold} or {\sl emphasize} something
```
se debería ver como “to be **bold** or _emphasize_ something”.

---
Las fuentes slanted e italic tienen el problema de que la ultima letra abarca parte del espacio después de este. Para corregir esto se puede usar el comando `\/` (corrección itálica). Por ejemplo: 
```tex
{\it italic\/} and {\sl slanted\/} words.
```

La regla de oro es usar `\/` justo cuando se cambia de italic o slanted a roman excepto cuando el siguiente carácter sea un punto o coma. Por ejemplo
```tex
{\it italics\/} for {\it emphasis}.
```

La puntuación no debe ser incluida en el cambio de fuente. En el caso de los signos de dos puntos (:) o punto y coma (;) se recomienda agregar la corrección itálica: `{\it word\/};`.

Todas las letras en todas las fuentes tiene una corrección itálica. En fuentes no inclinadas usualmente es cero. Una excepción son las negritas ya la letra f en negritas, se debe escribir como: `` `{\bf f\/}'``.

---
Cuando no se establece ninguna fuente se utilizará la primitiva `\nullfont` que indica una fuente sin caracteres.

---

$\TeX$ también puede manejar cambio de tamaños de la fuente. Por defecto usa una fuente de 10pt, pero es posible cambiarlo con los siguientes comandos:
- `\tenrm` para la fuente “$\rm roman$ de 10pts”
- `\ninerm` para la fuente “$\rm roman$ de 9pts”
- `\eightrm` para la fuente “$\rm roman$ de 8pts”
- `\sevenrm` para la fuente “$\rm roman$ de 7pts”
- `\sixrm` para la fuente “$\rm roman$ de 6pts”
- `\fiverm` para la fuente “$\rm roman$ de 5pts”
- `\tensl` para la fuente “slanted de 10pts”
- `\ninesl` para la fuente “slanted de 9pts”
-  etc.

En plain$\TeX$ no hay diferencia notable entre `\rm` y `\tenrm`. Sin embargo, `\rm` puede ser reescrito para que cambie la forma más no el tamaño de la fuente.

---
Se puede cargar cualquier fuente $\TeX$ siempre que existan los archivos y se tenga su nombre clave. Si estos requisitos se cumple, se puede usar el comando `\font` para cargar y asignar un comando a esa fuente. La sintaxis es la siguiente:
>[!abstract] Sintaxis
> `\font`⟨*c. sequence*⟩`=`⟨*font name*⟩ 

Por ejemplo el comando `\ninerm` está definida de la siguiente manera:
```tex
\font\ninerm=cmr9
```
Aquí `cmr9` es el nombre clave para la fuente “Computer Modern Roman de 9pts”.

> [!nota] 
> Las fuentes que se pueden usar en plain`\TeX` son de un formato especial. Se crean usando su programa hermano METAFONT.
> 
> Si se quiere usar fuentes modernas, se debe hacer uso de otros “engines” de $\TeX$ como XeTeX o LuaTeX. 

El comando `\font` también permite cambiar el tamaño de la fuente cargada mediante la palabra clave `at`. La sintaxis es la siguiente:
>[!abstract] Sintaxis
> `\font`⟨*c. sequence*⟩`=`⟨*font name*⟩` at `⟨*tamaño*⟩

Por ejemplo el siguiente comando:
```tex
\font\magnifiedfiverm=cmr5 at 10pt
```
carga la fuente “Computer Modern Roman de 5pts” y la estira para que sea de 10pt.

Otra forma de modificar el tamaño de una fuente es mediante la palabra clave `scaled` 
que sirve para escalar una fuente. La sintaxis es la siguiente:\
>[!abstract] Sintaxis
> `\font`⟨*c. sequence*⟩`=`⟨*font name*⟩` scaled `⟨*factor de escala*⟩

donde ⟨*factor de escala*⟩ es un entero que representa la escala multiplicada por 1000. Por ejemplo el comando:
```tex
\font\magnifiedfiverm=cmr5 scaled 2000
```
carga la fuente “Computer Modern Roman de 5pts” al doble de su tamaño.

Por motivos históricos, $\TeX$ proporciona algunos comandos que sirven como abreviaciones para escalas fijas:
- `\magstephalf` para una escala de $\sqrt{1.2}$.
- `\magstep`⟨$n = 0, 1, \ldots, 5$⟩ para una escala de $1.2^n$.
Por ejemplo el  comando
```tex
\font\bigtenrm=cmr10 scaled\magstep2
```
carga la fuente “Computer Modern Roman de 10pts” escalado en un factor de $1.2^2 = 1.44$.