---
title: Como guardar contraseñas de aplicaciones
date: 2024-10-06
draft: false
tags:
  - tecnologia
  - programacion
  - aplicaciones
  - sistemas
---
### Para ello se utilizan archivos especiales conocidos como ".env files"
### Donde además, se almacenan además de credenciales

## ¿Qué son?
Son archivos de nombre ".env" que contienen las configuraciones en texto plano, es decir, archivos en los cuales se definen variables y sus valores.

## ¿Qué importancia tienen?
Estos archivos juegan un rol súper importante a la hora de securizar los desarrollos de software. Se habla de securizar ya que la mayoría de configuraciones tienen relación con credenciales de acceso a diversos sistemas, como por ejemplo: bases de datos, APIs de terceros, etc.

Este archivo jamás es incluido en el repositorio en el cual se esté trabajando o se difunde con otros desarrolladores, para que estas configuraciones/credenciales no sean visualizadas por terceros, sin embargo, hay una copia del mismo que generalmente se le denomina como “envsample”, el cual posee las declaraciones de todas las variables, pero sin ser inicializadas. Esto es con el motivo de que otras personas puedan definir sus propias configuraciones y que el programa pueda ser ejecutado.

## Ejemplo del contenido del archivo "envsample"

```javascript
DATABASE_HOST=""
DATABASE_PORT=""
DATABASE_USERNAME=""
DATABASE_PASSWORD=""
```

## Ejemplo del contenido del archivo ".env"

```javascript
DATABASE_HOST="10.16.86.144"
DATABASE_PORT="3666"
DATABASE_USERNAME="usuarioDeBaseDeDatos"
DATABASE_PASSWORD="5sT8k)m.%.asijiHnLwnNF346"
```


## ¿Cómo se leen estos archivos?
Dentro de cada entorno de desarrollo existen distintos paquetes que permiten realizar esta tarea, a continuación se mencionan algunos:

* Dart: [dotenv](https://pub.dev/packages/dotenv)
* Flutter: [flutter_dotenv](https://pub.dev/packages/flutter_dotenv)
* Python: [python-dotenv](https://pypi.org/project/python-dotenv/)
* Java: [dotenv-java](https://github.com/cdimascio/dotenv-java)
* Go: [godotenv](https://github.com/joho/godotenv)

En algunas tecnologías, esto queda a cargo del desarrollador, como por ejemplo en BASH, PowerShell o scripts para CMD de Windows.

