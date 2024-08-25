# Las fuentes roman de $\TeX$
$\TeX$ tiene varias fuentes incorporadas. La fuente por defecto es “Modern Computer Roman”, esta contiene los siguientes símbolos:
- Letras de la “A” a la “Z” y de la “a” a la “z”.
- Los dígitos “0” a “9”.
- Signos de puntuación comunes: : ; ! ? ( ) [ ] ‘ ’ - * / . , @.
- Las ligaturas: ff, fi, fl, ffi, ffl, ``` `` ``` para “, `''` para ”, `` !` `` para ¡, `` ?` `` para ¿, `--` para –, `---` para —.
- Los símbolos + y =.

Para acceder al resto de caracteres visibles de [[ASCII]], para el resto hay que usar secuencias de control:
- `\$`, `\#`, `\%`, `\&` y `\_` para $, #, %, & y \_, respectivamente.
- ^ y ~ se obtienen como`\^{}` y `\~{}`, ya que funcionan como acentos.
- \\, {, }, |, < y >  son obtenibles en el modo matemático como `$\backslash$`, `$\{$`, `$\}$`, `$|$`, `$<$` y `$>$`, respectivamente.
- El carácter " funciona, pero es más aconsejable usar la ligatura `''`.

$\TeX$ también puede manejar algunos tipos de acentos mediante secuencias de control, estos son:
- Acento grave: `` \`o `` para ò.
- Acento agudo: `\'o` para ó.
- Acento circunflejo: `\^o` para ô
- Diéresis: `\"o` para ö.
- Virgulilla: `\~o` para õ.
- Macrón: `\=o` para ō.
- Acento punto: `\.o` para ȯ.
- Acento breve: `\u o` para ŏ.
- Carón: `\v o` para ǒ.
- Doble acento agudo: `\H o` para ő.
- Barra de corbata: `\t oo` para o͡o.

También, $\TeX$ proporciona tres tipos de acentos que van debajo:
- Cedilla: `\c o` para o̧.
- Punto inferior: `\d o` para ọ
- Macrón suscrito: `\b o` para o̱

Por último, $\TeX$ contiene algunas combinaciones de letras y letras especiales:
- `\oe` y `\OE` para œ y Œ.
- `\ae` y `\AE` para æ y Æ
- `\aa` y `\AA` para å y Å
- `\o` y `\O` para ø y Ø
- `\l` y `\L` para ł y Ł
- `\ss` para ß

Finalmente tenemos los símbolos ı y ȷ que corresponden a la i y j sin punto y pueden ser obtenidas mediante `\i` y `\j`. Estos son usados para cambiar el acento de la i y j.

Las mismas convenciones se aplican para las fuentes negrita, slanted e itálica, salvo que el símbolo de dólar ($) en itálica se verá como el símbolo de libra (£).

La fuente monoespaciada es ligeramente al resto de fuentes. La combinación de letras (fi, ffi, etc.) ya no son combinadas como ligaduras. También será posible usar lo caracteres `"`, `|`, `<` y `>`. Sin embargo, los acentos y letras especiales `\H`, `\.`, `\l` y `\L` ya no podrán ser usadas. 

También existen algunos símbolos cuya apariencia no depende de la fuente, dado que son tomados de la fuente matemática. Estos símbolos son:
- `\dag` para †
- `\ddag` para ‡
- `\S` para §
- `\P` para ¶

---
Los acentos en $\TeX$ son construidos mediante la primitiva `\accent`. La sintaxis es la siguiente
>[!sintaxis]
>`\accent`⟨_number_⟩⟨_character_⟩

donde ⟨_number_⟩ está entre 0 y 255 e indica la posición del acento en la fuente actual. Por ejemplo, el comando `\'` está definido como:
```tex
\def\'#1{{\accent"13 #1}}
```

Para los acentos, se asume que está posicionado para un carácter cuya altura es exactamente 1ex de la fuente actual. Sin embargo, los acentos se posicionarán de manera que se adaptan a la altura del carácter así como su inclinación.

La anchura de la construcción final, será la misma que la del carácter a la que se le aplica el acento, sin importar la anchura del acento.

Los comandos que son independientes del modo, como los de cambio de fuente, pueden aparecer entre ⟨_number_⟩ y ⟨_character_⟩, sin embargo, los caracteres de agrupación no pueden intervenir.

Si ningún carácter adecuado está presente, el acento aparecerá de la misma manera que si se hubiera usado el comando `\char`⟨_number_⟩. Por ejemplo `\'{}` producirá ´.
