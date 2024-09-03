# Modos

En $\TeX$ existen 6 diferentes modos, dependiendo de como se colocarán las cajas, estos son:
- **Modo vertical.** Cuando se construye la lista vertical principal, de la cual las se derivan las páginas de salida.
- **Modo vertical interno.** Cuando se construye una lista vertical para una `\vbox` (o `\vtop`).
- **Modo horizontal.** Cuando se construye una lista horizontal para un párrafo.
- **Modo horizontal restringido.** Cuando se construye una lista horizontal para una `\hbox`.
- **Modo matemático.** Cuando se construye una fórmula matemática que va a ser colocada en una lista horizontal.
- **Modo matemático de visualización.** Cuando se construye una fórmula matemática que va a ser colocado en su propia línea, interrumpiendo temporalmente el párrafo actual.

Por defecto, $\TeX$ estará en modo vertical y comandos como `\vskip` o `\hrule` mantienen el modo sin alterar. Sin embargo algunos comandos hacen que este modo se pause y se cambie a otro. Por ejemplo `\centerline` hace que el modo se cambie a horizontal restringido para crear una caja horizontal cuyo contenido está centrado, cuando esta caja ya ha sido procesada, se regresa al modo vertical y se agrega esa caja a la lista vertical.

Otro ejemplo es cuando se escriben párrafos, estos hacen que el modo sea cambiado a horizontal. Cuando el párrafo ha sido terminado, se crean las cajas correspondientes y el modo se cambia a vertical para colocarlas una debajo de la otra.

Para ser más específico, cuando se está en modo vertical o vertical interno, el primer token de un nuevo párrafo cambia el modo a horizontal hasta que el párrafo acabe. Esto ocurre cuando se escribe cualquier carácter, un `\char`, `\accent`, `\hskip`, `\␣`, `\vrule` o un math shift `$`.

Es posible decirle a $\TeX$ que cambie a modo horizontal manualmente, mediante `\indent` o `\noindent`. La única diferencia es que el primero agrega una caja vacía cuya anchura será el valor actual de `\parindent` (sangría) mientras que el segundo no.  Por defecto, el valor de `\parindent` es de 20 pt.

Cuando se cambia de modo vertical a horizontal cuando un párrafo inicia, el efecto es el mismo que haber usado `\indent`. Es posible agregar doble sangría usando `\indent\indent`, mientras que usar `\noindent\noindent` es igual que un solo `\noindent`. En el caso de `\indent` y `\noindent` mezclados, se ignorará el `\noindent`, por ejemplo `\indent\noindent` es equivalente a solo `\indent`.

 Casi siempre $\TeX$ estará en modo horizontal, mientras que entrará en modo vertical cuando los párrafos terminen. El [[Capitulo 8]] menciona que un párrafo termina cuando se agrega un `\par` o cuando existe una línea en blanco en el documento. Sin embargo, también es posible terminar un párrafo con ciertos comandos incompatibles en el modo horizontal, como `\vskip`.

El modo matemático inicia cuando se agrega el token `$` en el modo horizontal y termina cuando se encuentra con otro token `$`. Esto hace que se genera la fórmula matemática, sea agregue al párrafo actual y se regrese al modo horizontal.

Sin embargo, si se encuentra con dos tokens consecutivos `$$`, entonces $\TeX$ interrumpe el párrafo actual, agrega lo procesado a la lista vertical, luego procesa la fórmula matemática, que termina hasta el siguiente `$$`, en modo matemático de visualización, agrega la fórmula a la lista vertical, para finalmente regresar al modo horizontal para leer el resto del párrafo.

En el modo vertical (o vertical interno) los espacios, líneas en blanco o comandos `\par` son ignorados, por lo que no modifican el documento impreso. Sin embargo, el espacio de control `\␣` está restringido al modo horizontal, por  lo que iniciará un nuevo párrafo. Cuando `\␣` es usado como inicio de un párrafo, este agregará un espacio en blanco después de la sangría.

Al final de un manuscrito de $\TeX$, lo mejor es terminar con el comando `\bye` que es una abreviación para `\vfill\eject\end`. El comando `\vfill` hace que se cambie a modo vertical y se agrega suficiente espacio para rellenar el resto de la página, `\eject` hace que se genere la última página y finalmente `\end` le dice a la computadora que es el final de la rutina.

$\TeX$ entra en modo vertical interno cuando le pides que construya algo de una lista vertical de cajas usando `\vbox`, `\vtop`, `\vcenter`, `\valign`, `\vadjust` o `\insert`. Mientras que entra en modo horizontal restringido cuando le pides que construya algo de una lista horizontal de cajas usando `\hbox` o `\halign`.

El modo también afecta el efecto de algunos tokens. Por ejemplo, `\kern` especifica un espacio vertical en el modo vertical, mientras que especifica un espacio horizontal en modo horizontal. Un carácter de cambio matemático `$` causa que se entre en modo matemático en el modo horizontal, pero termina el modo matemático cuando se está en este modo. Lo mismo sucede con `$$`, en modo horizontal iniciará el modo matemático de visualización y en este modo lo terminará, pero además, cuando se está en modo horizontal restringido simplemente denotará una fórmula matemática vacía.

Es usual que $\TeX$ interrumpa los modos para hacer otra tarea en otro modo, como `\hbox{` que interrumpe cualquier modo para entrar en modo horizontal restringido hasta su cierre con su correspondiente `}` y se reanudará lo que sea que se haya estado haciendo antes de la interrupción. En estos casos, los cálculos son influenciados por el modo más reciente.

---
Para entender mejor los modos, se puede revisar el siguiente documento de prueba llamado `modes.tex`, el cual ejecuta todos los modos a la vez:
```tex
\tracingcommands=1
\hbox{
$
\vbox{
\noindent$$
x\showlists
$$}$}\bye
```
El primer comando `\tracingcommands=1` le dice a $\TeX$ que agregue al `.log` todos los comandos que recibe. Esto hace que en el documento `modes.log` aparezca el siguiente código:
```log
{vertical mode: \hbox}
{restricted horizontal mode: blank space  }
{math shift character $}
{math mode: blank space  }
{\vbox}
{internal vertical mode: blank space  }
{\noindent}
{horizontal mode: math shift character $}
{display math mode: blank space  }
{the letter x}
```

El significado es que $\TeX$ primero vio un token `\hbox` en modo vertical, lo que causó que siguiera adelante y además leyó de manera implícita el `{`.

Entonces $\TeX$ entro en modo horizontal restringido y vio el token de espacio en blanco que resultó del final de la línea 2 en el archivo. Luego vio un carácter de cambio matemático, aun en modo horizontal restringido, lo que causó que se cambiara a modo matemático y otro espacio apareció.

Entonces `\vbox` inicio un modo vertical interno y `\noindent` cambio a modo horizontal dentro de este. Dos signos `$` consecutivos causaron que se iniciara el modo matemático de visualización, aunque solo un signo `$` es mostrado por `\tracingcommands` ya que el primero causó que se buscara por el siguiente.

Lo siguiente en `modes.log` después de la salida de arriba es `{\showlist}`. Este es un comando de diagnóstico que se puede usar para ver las cosas que $\TeX$ normalmente se mantiene para si mismo. Este causa que $\TeX$ muestre las listas en las que está trabajando, en el modo actual y en todos los modos acumulados donde el trabajo ha sido suspendido:
```log
### display math mode entered at line 5
\mathord
.\fam1 x
### internal vertical mode entered at line 4
prevdepth ignored
### math mode entered at line 3
### restricted horizontal mode entered at line 2
\glue 3.33333 plus 1.66666 minus 1.11111
spacefactor 1000
### vertical mode entered at line 0
prevdepth ignored
```

En este caso, la lista representa cinco niveles de actividad, todos presentes al final de la línea 6 de `modes.tex`. El modo actual mos mostrado primero, el cual es el modo matemático de visualización, que inició en la línea 5. La lista matemática actual contiene un objeto “mathord”, que consiste en la letra `x` en la familia 1. 

Fuera del modo matemático de visualización, viene el modo vertical interno, al cual $\TeX$ regresará cuando el párrafo que contiene la formula visualizada sea completada. La lista vertical en ese nivel está vacía, `prevdepth ignored` significa que `\prevdepth` tiene un valor de $\leq -1000\,\text{pt}$, por lo que el próximo pegamento de interlineado será omitido.

El modo matemático fuera de este modo vertical interno tiene una lista vacía, sin embargo, el modo horizontal restringido que encierra el modo matemático contiene algo de pegamento.

Finalmente, vemos el modo vertical principal que encierra todo, esto se puede ver ya que dice `entered at line 0`, es decir, antes ed que el archivo `modes.tex` haya sido introducido, nada ha sido agregado hasta ahora a la lista vertical en este primer nivel.