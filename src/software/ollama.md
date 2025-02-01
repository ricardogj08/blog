# 🦙 Ollama

Es un software que permite instalar, configurar, ejecutar y gestionar ***modelos de lenguaje inteligente***, el cual también es un programa que recopila una gran cantidad de información en texto para entender como funciona el lenguaje humano y poder generar predicciones o continuación de oraciones.

> Dicho de otra manera, ***inteligencia artificial***.

## Instalación

* Puedes instalar `ollama` desde cualquier distribución GNU/Linux con el siguiente comando:

```
curl -fsSL https://ollama.com/install.sh | sh
```

> Probado correctamente en Slackware 15.0

* Comprueba su instalación consultando la versión de `ollama` instalado:

```
# Opción 1
ollama -v

# Opción 2
ollama --version
```

## Primeros pasos

* Muestra un mensaje de ayuda acerca de los comandos disponibles:

```
# Opción 1
ollama help

# Opción 2
ollama -h

# Opción 3
ollama --help
```

* Ejecuta el servicio o servidor de `ollama` (por defecto `127.0.0.1:11434`):

```
ollama serve
```

> Siempre debes ejecutar el servicio de `ollama` para utilizar cualquier modelo de lenguaje.

* Instala y ejecuta un modelo de lenguaje:

```
ollama run MODELO

# Ejemplo
ollama run deepseek-r1:7b
```

> Puedes encontrar un listado completo de modelos de lenguaje soportados por `ollama`: <https://ollama.com/search>

## Uso

* Descarga e instala un modelo de lenguaje:

```
ollama pull MODELO

# Ejemplo
ollama pull deepseek-r1:7b
```

> Lo que se encuentra del lado derecho de dos puntos `:` representa la versión o variante del modelo del lenguaje que selecciones.

* Lista todos los modelos de lenguaje instalados en el equipo:

```
# Opción 1
ollama list

# Opción 2
ollama ls
```

* Muestra información sobre un modelo de lenguaje instalado:

```
ollama show MODELO

# Ejemplo
ollama show deepseek-r1:7b
```

* Lista todos los modelos de lenguaje instalados en ejecución:

```
ollama ps
```

* Detiene la ejecución de un modelo de lenguaje instalado:

```
ollama stop MODELO

# Ejemplo
ollama stop deepseek-r1:7b
```

* Copia o duplica un modelo de lenguaje instalado:

```
ollama cp MODELO OTRO_MODELO

# Ejemplo
ollama cp deepseek-r1:7b mi_deepseek
```

> No afecta al modelo del lenguaje original instalado.

* Elimina un modelo de lenguaje instalado:

```
ollama rm MODELO

# Ejemplo
ollama rm deepseek-r1:7b
```

* Crea o importa un modelo de lenguaje personalizado a través de un archivo (por defecto `Modelfile`):

```
ollama create MI_MODELO

# También
ollama create MI_MODELO -f Modelfile
```
* Permite compartir un modelo lenguaje en un servidor remoto (por defecto `127.0.0.1:11434`):

```
ollama push MI_MODELO
```

> Permite compartir tus propios modelos de lenguaje con otras personas.
>
> Más información en: <https://github.com/ollama/ollama/blob/main/docs/import.md#Sharing-your-model-on-ollamacom>

## Referencias

* [Sitio web oficial de Ollama.](https://ollama.com/)
* [Repositorio oficial de Ollama.](https://github.com/ollama/ollama)
