# 01 - Instalación de MoonScript

> Esta es la primera parte de una serie de artículos o apuntes para aprender MoonScript.

[**MoonScript**](https://moonscript.org/) es un lenguaje de scripting dinámico que se compila en [Lua](https://www.lua.org/) (un lenguaje de programación de propósito general ligero y potente).

Cuando hablamos de lenguajes de scripting, existe un programa llamado interprete que lee una instrucción, valida si la sintaxis es correcta y la ejecuta, o lee una secuencia de instrucciones y las ejecuta una por una.<br>
MoonScript es dinámico, puede compilar toda una serie de instrucciones a Lua y después ejecutarse (`moonc`), o compilarse por instrucción y ejecutarse (`moon`).

Puedes utilizar toda la potencia de Lua en MoonScript con una sintaxis aún más simple y comprensible, además de características adicionales del propio lenguaje. La sintaxis es similar a Python, utiliza la indentación para delimitar bloques de instrucciones, si programas en este lenguaje será bastante sencillo aprender MoonScript. Si eres programador JavaScript, MoonScript es el [CoffeeScript](https://coffeescript.org/) para Lua.

> Para aprender MoonScript es indispensable conocer la sintaxis básica de Lua.

¿Y como instalo MoonScript?, para sistemas basados en Arch Linux solo debes ejecutar los siguientes comandos con privilegios de administrador:

```shell
# pacman -S gcc lua51 luarocks
# luarocks --lua-version 5.1 install moonscript
```

Se proporcionaran los ejecutables:

* `moon`
* `moonc`

Además del módulo `moonscript` para Lua, que permite utilizar directamente MoonScipt dentro de Lua o controlar algunos aspectos del proceso de compilación:

* `require 'moonscript'`

## Referencias a utilizar

* [Manual de referencia de MoonScript.](https://moonscript.org/reference/)
* [Página oficial de MoonScript.](https://moonscript.org/)
* [Página oficial de Lua.](https://www.lua.org/home.html)
* [Manual de referencia de Lua 5.1.](https://www.lua.org/manual/5.1/es/)
