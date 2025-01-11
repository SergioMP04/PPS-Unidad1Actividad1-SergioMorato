# Informe de Actividad: Puesta en Producción Segura - Unidad 1

## Índice

- [Informe de Actividad: Puesta en Producción Segura - Unidad 1](#informe-de-actividad-puesta-en-producción-segura---unidad-1)
  - [Índice](#índice)
  - [Introducción](#introducción)
  - [Objetivos](#objetivos)
  - [Desarrollo de la Actividad](#desarrollo-de-la-actividad)
    - [3.1. Preparación del Entorno en Linux](#31-preparación-del-entorno-en-linux)
    - [3.2. Lanzamiento del Contenedor Docker](#32-lanzamiento-del-contenedor-docker)
    - [3.3. Instalación de Extensiones en Visual Studio](#33-instalación-de-extensiones-en-visual-studio)
    - [3.4. Pruebas del Entorno de Desarrollo](#34-pruebas-del-entorno-de-desarrollo)
  - [Conclusiones](#conclusiones)

---

## Introducción

En esta actividad se busca consolidar los conocimientos sobre entornos de desarrollo seguros y eficientes, utilizando herramientas modernas como Docker y Visual Studio. Se llevará a cabo la configuración de un entorno de desarrollo basado en contenedores, la instalación de extensiones en el IDE y la prueba de proyectos de código fuente para asegurar el funcionamiento adecuado de las herramientas instaladas.

---

## Objetivos

1. Crear un entorno de desarrollo Eclipse utilizando Docker.
2. Instalar extensiones en un IDE que permitan mejorar la productividad y seguridad del código.
3. Probar el entorno de desarrollo mediante la compilación y depuración de proyectos.

---

## Desarrollo de la Actividad

### 3.1. Preparación del Entorno en Linux

1. **Creación de las carpetas necesarias:**
   ```bash
   sudo mkdir -p $HOME/docker/eclipse/datos
   sudo chown -R $(whoami) $HOME/docker/eclipse
   sudo chgrp -R $(whoami) $HOME/docker/eclipse
   ```
   Estos comandos crean las carpetas donde se almacenarán los datos del contenedor y otorgan los permisos necesarios al usuario actual.
<p align="center">
    <img src="imagenes\Carpetas.png" alt="Config Nano">
</p>
2. **Configuración del entorno gráfico:**
   ```bash
   export DISPLAY=:0
   startxwin -- -listen tcp &
   xhost +
   ```
   - `export DISPLAY=:0`: Configura la pantalla para las aplicaciones gráficas.
   - `startxwin`: Inicia el servidor X para soporte gráfico.
   - `xhost +`: Permite conexiones al servidor X.
<p align="center">
    <img src="imagenes\Display.png" alt="Config Nano">
</p>
### 3.2. Lanzamiento del Contenedor Docker

Se utiliza el siguiente comando para iniciar el contenedor Docker con Eclipse:

```bash
sudo docker run -ti --rm \
  -e DISPLAY=$DISPLAY \
  -e artifactory_host='127.0.0.1:9999' \
  --name eclipse \
  -v /tmp/.X11-unix:/tmp/.X11-unix \
  -v $(pwd):/workspace \
  -v $HOME/docker/eclipse/datos:/home/developer \
  dockeruc/eclipse
```

<p align="center">
    <img src="imagenes\ErrorEclipse.png" alt="Config Nano">
</p>

En intentado ejecutarlo y de todas formas da error.
**Explicación del comando:**
- `-ti`: Permite interactuar con el terminal del contenedor.
- `--rm`: Elimina el contenedor al finalizar su ejecución.
- `-e DISPLAY=$DISPLAY`: Habilita el soporte gráfico.
- `-v /tmp/.X11-unix:/tmp/.X11-unix`: Monta el socket de X11 para aplicaciones gráficas.
- `-v $(pwd):/workspace`: Monta el directorio actual como espacio de trabajo.
- `-v $HOME/docker/eclipse/datos:/home/developer`: Monta la carpeta de datos del usuario dentro del contenedor.
- `dockeruc/eclipse`: Imagen Docker utilizada.

### 3.3. Instalación de Extensiones en Visual Studio

Para tener una mayor familiarización con el entorno se ha decidido escribir el Readme, en VScode con algunas extensiones instaladas para facilitar el trabajo, se adjuta captura del entorno.

   <p align="center">
      <img src="imagenes\Entorno.png" alt="Config Nano">
   </p>

1. **Extensiones instaladas:**
Estas extensiones principalmente nos ayudarán a desarrollar un código mucho más seguro.
   - **Checkstyle:** Ayuda a garantizar que el código cumple con estándares de estilo definidos.
   - **SonarLint:** Identifica problemas de calidad y vulnerabilidades en el código en tiempo real.
**En caso de Visual Studio los nombres cambiarán**
1. **Proceso de instalación en Visual Studio:**
   - Ve a `Extensions > Manage Extensions`.
   <p align="center">
      <img src="imagenes\Ext1.png" alt="Config Nano">
   </p>
   - Busca "Checkstyle" y "SonarLint" en la sección de "Online".
   <p align="center">
      <img src="imagenes\Ext2.png" alt="Config Nano">
   </p>
   - Haz clic en "Download" e instala las extensiones.
   - Reinicia Visual Studio para aplicar los cambios.
        <p align="center">
          <img src="imagenes\Ext4.png" alt="Config Nano">
        </p>
2. **Extensiones para programadores**
   En esta mini sección adjunto algunas extensiones que útilizo en mi día a día en la hora del desarrollo en Python.
   - Python
   - Python Extension Pack
   - Error Lens
      <p align="center">
          <img src="imagenes\Ext5.png" alt="Config Nano">
        </p>


### 3.4. Pruebas del Entorno de Desarrollo

1. **Descarga del código fuente:**
   - Usaré un código fuente proporcionado por el profesor, crearé un archivo llamado [main.py](source\main.py), dentró de la carpeta source para tener todo más ordenado.

2. **Importación del proyecto en Visual Studio:**
   - En este caso simplemnte nos dirigiremos a donde tengamos el archivo en mi caso a la carpeta source.
      <p align="center">
          <img src="imagenes\main.png" alt="Config Nano">
        </p>
3. **Compilación y ejecución:**
   - En mi caso ya tengo instalados tos los paths y lo unico que hay que hacer para que el código cocompile será darle al play.
      <p align="center">
          <img src="imagenes\run.png" alt="Config Nano">
          <img src="imagenes\funcionando.png" alt="Config Nano">
        </p>
4. **Depuración:**
   - Usaremos SonarCube para que nos diga los diferentes errores, para hacer pruebas he decidido ejecutarlo y ver lo qwue me daba el unico falo ha sido este.
   - A parte he decidico añadir yo un error al código en un try Except para que sea más visual la forma en la que márca que una variable no está siendo usada a lo largo del código.
   <p align="center">
      <img src="imagenes\SonarPy.png" alt="Config Nano">
      <img src="imagenes\e.png" alt="Config Nano">
   </p>

   - Aqui podremos ver como nos márca el error y nos da una salida con diferentes soluciones la propia extensión.
---

## Conclusiones

La configuración de un entorno de desarrollo basado en contenedores con Eclipse y Docker permite trabajar de forma segura y aislada, facilitando la gestión de dependencias y configuraciones. La integración con Visual Studio y las extensiones como Checkstyle y SonarLint proporcionan herramientas potentes para mantener la calidad y seguridad del código. Este ejercicio no solo refuerza habilidades técnicas, sino que también destaca la importancia de buenas prácticas en el desarrollo de software.

---

**Autor**
Sergio Morato Prieto

---


