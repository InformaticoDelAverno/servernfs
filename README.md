# servernfs
Este repositorio contiene todo lo necesario para poder construir una imagen de Docker que despliega un servidor NFS y sirve una ruta ("**/shared**") a través de dicho protocolo.

## Generar la imagen

```sh
$ docker build ./dockerFile -t servernfs
```

## Arrancar un contenedor

Si has construido localmente la imagen, el comando que debes utilizar es:
```sh
$ docker run --privileged -d -p 111:111 -p 2049:2049 --name servernfs -v /path/local:/shared servernfs
```
Si no quieres construirte la imagen, puedes utilizar una ya construida que se encuentra publicada en [Docker hub](https://hub.docker.com/r/informaticodelaverno/servernfs).

Para crear un contenedor de esta imagen  (Compartiendo "**/path/local**" vía NFS), hay que ejecutar:
```sh
$ docker run --privileged -d -p 111:111 -p 2049:2049 --name servernfs -v /path/local:/shared informaticodelaverno/servernfs
```
## Montar el directorio expuesto por el contenedor
El contenedor siempre expone el path "**/shared**", así que para montar ese path el comando a lanzar es el siguiente:
```sh
$ mount IPDelContenedor:/shared /path/local/de/montaje
```
For example:
```sh
$ mount 172.17.0.2:/shared /path/local/de/montaje
```
También es posible montar directamente un subdirectorio compartido que tiene como directorio principal el directorio compartido por el contenedor
```sh
$ mount IPDelContenedor:/shared/subdir /path/local/de/montaje
```
For example:
```sh
$ mount 172.17.0.2:/shared/subdir /path/local/de/montaje
```
# Notas
- "**nfs-kernel-server**" tiene que estar instalado para habilitar el soporte NFS.
- Es necesario cargar el módulo nfs en el kernel si no lo está ya.
```sh
$ modprobe nfs
```
