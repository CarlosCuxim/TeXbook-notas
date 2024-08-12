# Impresión de libros versus escritura ordinaria

Las computadoras tienen un conjunto limitado de caracteres (como el [[ASCII]]) por ejemplo, sin embargo en la impresión existen muchos más. $\TeX$ permite solventar estas diferencias.

Por ejemplo las computadoras solo tienen 3 tipos de comillas que son `` ` ``, `'`, y `"`, mientras que en los libros hay muchos mas. Para escribir las comillas en $\TeX$ se usa la siguiente construcción:
```tex
``I understand''
```
que debería verse como: “I understand”.

Los símbolos que se usan en el manual y para escribir código son los siguientes (incluye el espacio en blanco):
```
ABCDEFGHIJKLMNOPQRSTUVWXYZ
abcdefghijklmnopqrstuvwxyz
0123456789"#$%&@*+-=,.:;?!
()<>[]{}`'\|/_^~ 
```

Otro ejemplo son los símbolos de tipo guion:
- guion (-) `-`
- en-dash (–)  `--`
- em-dash (—) `---`
- signo de menos ($-$) `$-$`

También están las [[ligaturas]], que se hacen automáticamente, estas están programadas en la fuente. También están programadas en la fuente cosas como el [[kerning]].

---
Si no se tiene los símbolos, $\TeX$ tiene definido algunos comandos para estos, por ejemplo:
- `\lq` para `` ` `` (ver [[Control sequences#lq]])d
- `\rq` para `'` (ver [[Control sequences#rq]])

---
Algunas construcciones no funcionan por las ligaturas, como una comilla simple seguida de dobles comillas, para eso se usa el comando `\thinspace` (ver [[Control sequences#thinspace]]):
```tex
``\thinspace`I understand\thinspace"
```
se debería ver como “‘I understand’”.
