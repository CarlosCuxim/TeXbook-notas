# Controlando TeX

Para usar $\TeX$ haremos uso de las [[Control sequences|secuencias de control]], estos son comandos que se podrán usar mediante un carácter especial llamado [[Catcode#Escape character|carácter de escape]] que usualmente será `\`.

La sintaxis de las secuencias de control (comandos) usualmente será la siguiente
>[!note] Sintaxis
> ⟨*esc char*⟩⟨*cs name*⟩

donde:
- ⟨*esc char*⟩ es el carácter de escape.
- ⟨*cs name*⟩ es:
	- Una sucesión de letras.
	- Un carácter no letra.

Un ejemplo es el comando `\input` que se usa de la siguiente manera:
```tex
\input MS
```
que buscará y leerá un archivo llamado `MS.tex`.

Otro ejemplo son los comandos `\'` y  `\"` que permiten agregar acentos a las letras, por ejemplo
```tex
George P\'olya and Gabor Szeg\"o
```
debería verse como “George Pólya and Gabor Szegö”.

Cuando el comando es del primer tipo, será llamada una _palabra de control_. El comando empieza con la primera letra y termina justo antes del primer carácter que no sea una letra. Los espacios después de una palabra de control serán ignorados.

Cuando un comando es del segundo tipo, será llamado un _símbolo de control_. Siempre tienen exactamente un carácter de longitud (sin contar el carácter de escape). Los espacios después de un símbolo de control nunca serán ignorados.

---
Si se desea un espacio después de una palabra de control se deberá usar el comando `\␣`. No importa la cantidad de espacios después del comando, ya que espacios consecutivos son tratados como un solo espacio.

Por ejemplo, si el comando `\TeX` es usado para imprimir el logo $\TeX$, entonces
```tex
\TeX ignore spaces after control words
```
se debería ver como: “$\TeX$ignore spaces after control words”. Mientras que
```tex
\TeX\ ignore spaces after control words.
```
se debería ver como: “$\TeX$ ignore spaces after control words”.

Si se desean más de un espacio consecutivo se debe usar `\␣` multiples veces. Por ejemplo, tres espacios consecutivos se deberá escribir como `\␣\␣\␣`.

> [!nota]
> Otros caracteres de tipo espacio como ⟨return⟩ o ⟨tab⟩ (que denotaremos usando los símbolos ⏎ y ↹, respectivamente) también pueden generar caracteres de escape. Estos comandos (`\⏎` y `\↹`) usualmente tendrán el mismo significado que `\␣`.

---
Otros ejemplos de comandos, son los usados para los símbolos en formulas matemáticas:
- `\pi` para $\pi$
- `\Pi`  para $\Pi$
- `\aleph` para $\aleph$
- `\infty` para $\infty$
- `\le` para $\le$
- `\ge` para $\ge$
- `\ne` para $\ne$
- `\oplus` para $\oplus$
- `\otimes` para $\otimes$

> [!nota]
> Como se puede observar, los comandos son *case sensitive*, i.e., no hay relación entre las mismas palabras pero cambiando algunas mayúsculas o minúsculas.

---
$\TeX$ tiene internamente cerca de 900 comandos listos para ser usados. Pero existen formas de definir más.

De estos comandos, cerca de 300 son llamadas *primitivas*, estos son comandos internos del propio programa y no se descomponen en comandos más simples.

 Se pueden distinguir las primitivas y los comandos compuestos usando el comando `\show` seguido del comando.

Por ejemplo `\show\thinspace` hace que en la consola y log se muestre el siguiente texto:
```tex
> \thinspace=macro
->\kern .16667em .
```

Llamaremos como “plain $\TeX$” al conjunto de casi 600 comandos ya integrados dentro del programa más los casi 300 comandos primitivos.