+++
date = '2026-10-13T11:44:53Z'
draft = false
title = '📦 VirtualBox en Archlinux'
tags = ["linux", "virtualbox", "archlinux"]
summary = 'Instalar virtualbox en archlinux.'
+++

> [!IMPORTANT]
Si quieres obtener información y una explicación más detallada, visita la [Arch Wiki](https://wiki.archlinux.org/title/VirtualBox_(Espa%C3%B1ol)).

## Instalar VirtualBox

Los paquetes que debes instalar son:

- `virtualbox`
- `virtualbox-host-dkms`: proporciona módulos al sistema anfitrión.
- `linux-headers`: paquete de encabezado para el kernel, para compilar los módulos de VirtualBox.

Por defecto al instalar el paquete `virtualbox`, también se instala el paquete `virtualbox-host-dkms`.

```bash
sudo pacman -S virtualbox linux-headers
```

## Cargar los módulos del kernel de VirtualBox

Los módulos de VirtualBox se cargan automáticamente al momento del arranque, gracias a `systemd-modules-load.service`.

Por lo que para cargar los módulos de VirtualBox, reinicia el sistema o cargalos manualmente.

Para cargar un módulo manualmente después de la instalación de VirtualBox, ejecutamos.

```bash
sudo modprobe vboxdrv
```
El módulo `vboxdrv` se debe cargar obligatoriamente antes de ejecutar cualquier máquina virtual.

Otros módulos:

- `vboxnetadp`: para crear la interfaz del sistema anfitrión.
- `vboxnetflt`: para iniciar una máquina virtual usando esa interfaz de red.

También podemos cargar los módulos de VirtualBox manualmente ejecutando `vboxreload`.

```bash
sudo vboxreload
```

## Añadir tu usuario al grupo vboxusers

Para usar todas las funciones de virtualbox con nuestro usuario, debemos añadir nuestro usuario al grupo `vboxusers`.

```bash
sudo usermod -aG vboxusers $USER
``` 

Para que los cambios surtan efecto cierra sesión o reinicia el sistema.

Comprueba los cambios ejecutando:

```bash
groups && grep vboxusers /etc/group
```

## Extension Pack

El paquete de extensiones es un conjunto de funcionalidades adicionales para Virtualbox, sin embargo no es necesario para el correcto funcionamiento del mismo.

Aun así deberás instalarlo si vas a utilizar las siguientes funcionalidades que proporciona:

- Integración con Oracle Cloud Infrastructure

- Controlador de Host USB 2.0 y USB 3.0

- Cámara Web del Host

- VirtualBox RDP

- ROM PXE

- Cifrado de disco

- NVMe

Hay un paquete disponible en el AUR llamado `virtualbox-ext-oracle`, sin embargo yo suelo descargarlo directamente desde el sitio web de [VirtualBox](https://www.virtualbox.org/wiki/Downloads).

Ya sea dándole en el **boton de descarga**, o usando **wget**:

```bash
wget [url/de/descarga/vbox-extpack]
```

### Instalar Extension Pack

La instalación se puede hacer a través de la interfaz gráfica o usando `VBoxManage`.

La instalación del paquete de extensiones requiere **acceso root**. En la [Arch Wiki](https://wiki.archlinux.org/title/VirtualBox_%28Espa%C3%B1ol%29#Paquete_de_extensiones) se menciona el uso de herramientas como [Polkit](https://wiki.archlinux.org/title/Polkit) para otorgar acceso privilegiado a VirtualBox.

Yo instalaré el extensión pack usando `sudo`

```bash
sudo VBoxManage extpack install [/ruta/del/extension/pack.vbox-extpack]
```

Aceptas los términos y condiciones y listo.

## Fin

En principio con esto ya puedes empezar a crear tus máquinas virtuales y darles duro contra el muro, y si no, recuerda que tienes a tu disposición la [Arch Wiki](hhttps://wiki.archlinux.org/title/VirtualBox_%28Espa%C3%B1ol%29).

![maxwell-gif](./maxwell.gif "El gato maxwell gif")
