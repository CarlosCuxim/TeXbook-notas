# Agrupamiento

Los caracteres `{` y `}` tienen una función especial en $\TeX$. Estos permiten crear “bloques” donde los comandos tiene un efecto local. También puede ser usado para romper ligaturas, para evitar que los espacios consecutivos se combinen o que se eliminen después de un comando.

Sin embargo, el uso principal en $\TeX$ es para delimitar el argumento de algunos comandos. Por ejemplo, el comando:
```tex
\centerline{This information should be centered.}
```
Creará una línea centrada con la frase “This information should be centered”. Se pueden anidar cualquier número de grupos, como en:
```tex
\centerline{This information should be {\it centered}.}
```

>[!nota]
> Notar que el primer grupo sirve como argumento, mientras que el segundo funciona como un bloque. La diferencia es que el primer comando _requiere_ un argumento, pero el segundo _cambia_ uno o varios comando.

---
Una forma de aplicar cambios globales adentro de un grupo es mediante el comando `\global`. Por ejemplo, escribiendo
```tex
{\global\advance\count0 by 1}
```
incrementará el registro `\count0` en una unidad y el cambio se preservará aun después de que se sale del grupo. Notar que `\global` se salta todos los grupos, no solo el primero en el que está contenido.

---
Otra forma de crear grupos, además de los caracteres `{` y `}` es mediante los comandos `\begingroup` y `\endgroup`. Su principal utilidad es que estos comandos pueden estar dentro del texto de reemplazo en el comando `\def`, a diferencia de los corchetes. 

Notar que estos dos métodos para crear grupos no se pueden mezclar, por ejemplo, el siguiente código no es válido:
```tex
{ \begingroup } \endgroup % Error!
```