+++
date = '2026-10-05T11:47:50+02:00'
draft = false
title = '🐍 Bash Modo Vim'
tags = ["linux", "bash", "vim"]
summary = 'Bash con resaltado de sintaxis y modo de edición de vim.'
+++

## ¿Qué es ble.sh?

[Bash Line Editor](https://github.com/akinomyoga/ble.sh) es un editor de línea de comandos escrito en bash puro.

Con ```ble.sh``` podemos agregar a ```bash``` las siguientes características:

- Resaltado de sintaxis.
- Autocompletado mejorado.
- Modo de edición de Vim.

## Instalación y configuración

> [!IMPORTANT]
Los pasos aquí descritos corresponden a la documentación oficial de [ble.sh](https://github.com/akinomyoga/ble.sh#1-usage)

### Instalación

1. Clonamos el repositorio:

```bash
git clone --recursive https://github.com/akinomyoga/ble.sh.git
cd ble.sh
``` 

2. Instalamos ```ble.sh``` en nuestro sistema con el comando ```make```.

```bash
make install
```

Esto instalará **blesh** en ```~/.local/share/blesh/``` y ```~/.local/share/doc/blesh/```

3. Para cargar ```ble.sh``` por defecto en sesiones interactivas de **bash**, agregamos las siguientes líneas en nuestro ```.bashrc```.

```bash
# bashrc

# Agrega estas líneas al principio de .bashrc:
[[ $- == *i* ]] && source -- /path/to/blesh/ble.sh --attach=none

# Aquí van los ajustes de tu archivo bashrc...

# Agregue esta línea al final de .bashrc:
[[ ! ${BLE_VERSION-} ]] || ble-attach
```

> [!IMPORTANT]
Reemplaza ```/path/to/blesh/ble.sh``` por la ruta en donde se encuentra instalado ```ble.sh```. Si no cambiaste la ruta de instalación durante la compilación, la ruta es ```~/.local/share/blesh/ble.sh```.

### Configuración

Para configurar ciertas características de ```ble.sh``` tenemos que crear el fichero ```.blerc``` en el directorio **home** de nuestro usuario.

```.blerc``` es un script Bash que se ejecuta durante la carga de **blesh**.

```bash
touch ~/.blerc
```

**Blesh** proporciona una plantilla del fichero de configuración llamada [blerc.template](https://github.com/akinomyoga/ble.sh/blob/master/blerc.template)

Mi configuración es esta:

```bash
#!/bin/bash

# deshabilitar el autocompletado
bleopt complete_auto_complete=
# deshabilitar los marcadores "[ble: ...]"
bleopt prompt_eol_mark=''
bleopt edit_marker=
bleopt edit_marker_error=
# habilitar el modo vim
set -o vi

# configuración del modo vim
function blerc/vim-mode-hook {
        # desactivar la información del modo actual
        bleopt keymap_vi_mode_show=

        # Forma del cursor para cada modo
        ble-bind -m vi_nmap --cursor 2
        ble-bind -m vi_imap --cursor 5
        ble-bind -m vi_omap --cursor 4
        ble-bind -m vi_xmap --cursor 2
        ble-bind -m vi_cmap --cursor 0

        # Descartar la línea con C-c
        ble-bind -m vi_imap -f 'C-c' discard-line
        ble-bind -m vi_nmap -f 'C-c' discard-line

        # agregando funcionalidades de vim-surround
        ble-import vim-surround
}
blehook/eval-after-load keymap_vi blerc/vim-mode-hook
```

Con esta configuración:

- Deshabilito el autocompletado.

- Deshabilito los marcadores tipo ```[ble: ...]```.

- Habilito el modo vim.

- Establezco la forma del cursor en cada modo.

- ```CTRL``` + ```C``` para descartar la línea.

- Agrego funcionalidades de vim-surround

Para obtener información más detallada sobre que **funciones** puedes deshabilitar y sobre la configuración del **modo vi(vim)**, tienes a tu disposición la **documentación oficial**.

- [Deshabilitar funciones](https://github.com/akinomyoga/ble.sh#22-disable-features)

- [Modo Vi(Vim)](https://github.com/akinomyoga/ble.sh/wiki/Vi-(Vim)-editing-mode)

#### Establecer tiempo de espera para ESC.

Notarás cierto retraso al cambiar de modos en el **modo vi(vim)**.

Para reducir el tiempo de espera, agregamos lo siguiente en alguna parte de nuestro ```~/.bashrc```:

```bash
# Tiempo de espera de ESC - ble.sh
stty time 0 # tiempo de espera en el pseudo-terminal handler, configurado mediante stty
bind 'set keyseq-timeout 1' # tiempo de espera para Readline en bash
```

## Fin

Esto sería todo de mi parte, no olvides revisar la [documentación oficial de blesh](https://github.com/akinomyoga/ble.sh) para entender todo con más detalle.
