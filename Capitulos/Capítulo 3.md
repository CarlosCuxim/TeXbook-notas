# Controlando TeX

Para usar $\TeX$ haremos uso de las [[Control sequences]], estas son comandos que se podrán usar mediante un carácter especial llamado [[Escape character]] que usualmente será `\`.

La sintaxis de las secuencias de control usualmente será la siguiente ⟨*esc char*⟩⟨*cs name*⟩ donde:
- ⟨*esc char*⟩ es el carácter de escape.
- ⟨*cs name*⟩ es:
	- Una sucesión de letras.
	- Un carácter no letra.

Un ejemplo es el comando `\input` que se usa de la siguiente manera:
```tex
\input MS
```
que buscará y leerá un archivo llamado `MS.tex`.