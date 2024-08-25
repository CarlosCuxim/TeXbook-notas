# Dimensiones

$\TeX$ puede manejar  varias unidades de medida, estos se identificarán usando una abreviación de dos caracteres. Las unidades de medida permitidas son:
- `pt` para el punto tipográfico.
- `pc` para la pica (1 pc = 12 pt).
- `in` para las pulgadas (1 in = 72.27 pt)
- `bp` para el big point (72 bp = 1 in)
- `cm` para el centímetro (2.54 cm = 1 in)
- `mm` para el milímetro (10 mm = 1 cm)
- `dd` para el punto Didot (1157 dd = 1238 pt)
- `cc` para el cícero (1 cc = 12 dd)
- `sp` para el scaled point (65536 sp = 1 pt)

La sintaxis exacta para las dimensiones, que representaremos por ⟨_dimension_⟩, será:
> [!sintaxis]
> ⟨_optional sign_⟩⟨_number_⟩⟨_unit of measure_⟩
> ⟨_optional sign_⟩⟨_digit string_⟩`.`⟨_digit string_⟩⟨_unit of measure_⟩

donde ⟨_optional sign_⟩ es `+`, `-` o nada, mientras que ⟨_digit string_⟩ consiste en cero o más dígitos decimales consecutivos. El `.` en la segunda forma también puede ser una `,`.

Por ejemplo, seis dimensiones válidas podrían ser: `3 in`, `29 pc`, `-.013837in`, `+ 42,1 dd`, `0.mm` o `123456789sp`. Los espacios son opcionales antes de los signos, el número y las unidades de medida y después de la dimensión, sin embargo  no se permite espacios  entre los dígitos del número o entre las letras de la unidad de medida.

De manera interna, todas las dimensiones son expresadas como enteros de la unidad `sp`. Esto permite que los resultados sean exactamente los mismos independientemente de la computadora usada.

De esta forma $\TeX$ no puede manejar dimensiones cuyo valor absoluto sea igual o mayor a $2^{30}$ sp. Esto hace que la mayor dimensión posible sea poco menos que 16384 pt, 12.892 ft o 2.7583 m.

>[!nota]
> Cuando la dimensión sea 0, aunque la unidad sea irrelevante, aun así debe indicarse alguna unidad, como `0pt`.

---
El comando `\magnification` permite cambiar el tamaño de todo el archivo. De este modo, el código
```tex
\magnification=1200
```
hará que todo el texto sea agrandado en un 20%. En general a `\magnification` se le puede agregar cualquier valor ⟨_number_⟩, donde este representa 1000 veces el ratio de crecimiento (o reducción). En el valor ⟨_number_⟩ se pueden usar los comandos `\magstep` y `\magstephalf`.

La única limitación es que el comando debe estar presente antes de que la primera página sea creada. No se pueden aplicar diferentes magnificaciones al mismo documento.

Cuando se cambia el valor `\magnification` todas las unidades son cambiadas por el factor correspondiente. De este modo una magnificación de 2000 hará que `1cm` sea en realidad `2cm`. Sin embargo se pueden especificar dimensiones que sean independientes de la magnificación mediate la palabra clave `true` antes de la unidad, puede haber un espacio entre estos dos. Por ejemplo `\vskip0.25truecm` siempre agregará un espacio de 0.25 cm sin importar la magnificación. Otro ejemplo es `\hsize` que cuando se usa el comando `\magnification` se redefine como:
```tex
\hsize = 6.5 true in
```
Esto permite que el ancho de columna siempre sea el mismo independientemente de la magnificación.

La magnificación es gobernada por la primitiva `\mag` el cual es un parámetro entero que debe ser positivo y a lo más 32768. El valor de `\mag` es examinado en tres casos
1. Justo antes de que la primera página haya sido enviada al archivo `.dvi`
2. Cuando se calculan las dimensiones `true`
3. Cuando el archivo `.dvi` es cerrado.

Algunas implementaciones de $\TeX$ producen salida que no es `.dvi`, por lo que examinan el valor de `\mag` en el caso 2 y cuando se envía cada página. Dado que solo se puede colocar una magnificación, el valor de`\mag` no se debe cambiar después de que se haya examinado por primera vez.

---
$\TeX$ también reconoce dos unidades de medida relativas (en vez de absolutas). Estos son:
- `em` que es la medida de un “quad” en la fuente actual.
- `ex` que es la “altura de la x” en la fuente actual.

Cada fuente define su propio valor `em` y `ex`. Aproximadamente un `em` tiene la anchura del carácter “M”, mientras que un `ex` tiene la altura del carácter “x”, sin embargo esta no es una regla estricta.

En la fuente default de $\TeX$ un em dash (—) tiene `1 em` de anchura, los dígitos tienen medio em de anchura y el carácter x tiene exactamente la altura de `1 ex`. Más específicamente:
- En la fuente roman (`\rm`): 1 em = 10 pt y 1 ex ≈ 4.3 pt
- En la fuente negritas (`\bf`): 1 em = 11.5 pt  y 1 ex ≈ 4.44 pt
- En la fuente monoespaciada (`\tt`): 1 em = 10.5 pt y 1 ex ≈ 4.3 pt

Como regla de oro, las unidades em son usadas en longitudes horizontales que dependan de la fuente, mientras que la unidades ex para longitudes verticales que dependan de la fuente.

---
Los nombres de unidades otras palabras clave no son case sensitive, es decir, se pueden mesclar minúsculas con mayúsculas, como `Pt` y `PT` para `pt`. De igual manera el código de categoría es irrelevante.