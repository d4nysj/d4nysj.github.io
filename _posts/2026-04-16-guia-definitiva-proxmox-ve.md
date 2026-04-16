---
layout: post
title: "Proxmox VE — Guía de Instalación y Configuración Desde Cero"
date: 2025-01-01
categories: [homelab, virtualización]
tags: [proxmox, kvm, lxc, servidor, linux]
description: "Guía completa para montar un servidor de virtualización profesional con Proxmox VE, desde la instalación hasta la optimización avanzada."
toc: true
---

**Proxmox VE** es un hipervisor bare-metal de código abierto que combina virtualización KVM con contenedores LXC, gestionado desde una potente interfaz web. Esta guía cubre todo el proceso: desde preparar el USB hasta optimizar el sistema con scripts, alias y herramientas de monitorización.

---

## ¿Qué es Proxmox VE?

Proxmox Virtual Environment es un hipervisor de **tipo 1** (bare-metal): se instala directamente sobre el hardware sin sistema operativo previo, lo que le da un rendimiento prácticamente idéntico al hardware físico.

| Característica | Detalle |
|---|---|
| Tipo de hipervisor | Tipo 1 — acceso directo al hardware |
| Tecnologías | KVM (VMs) + LXC (contenedores) |
| Interfaz | Web UI accesible desde cualquier navegador |
| Licencia | Open Source (AGPLv3) |
| Coste | Gratuito — la suscripción es opcional |

---

## Requisitos mínimos

- **CPU:** 64 bits con Intel VT-x o AMD-V activado en BIOS
- **RAM:** 2 GB mínimo (recomendados 8 GB o más)
- **Almacenamiento:** 32 GB mínimo, SSD recomendado
- **Red:** Cable Ethernet (imprescindible durante la instalación)
- **USB:** Pendrive de al menos 2 GB

> Un ordenador antiguo o portátil viejo son perfectamente válidos para empezar.

---

## Fase 1 — Preparar el USB de instalación

### Descargar la ISO

Ve a [proxmox.com/downloads](https://www.proxmox.com/en/downloads) y descarga la última versión estable de **Proxmox VE** (no "Backup Server" ni "Mail Gateway").

### Flashear con Balena Etcher

1. Descarga [Balena Etcher](https://etcher.balena.io)
2. Selecciona la ISO con **"Flash from file"**
3. Elige tu pendrive en **"Select target"** — comprueba que no es otro disco
4. Pulsa **"Flash!"** y espera a que termine

---

## Fase 2 — Instalación en el servidor

### Arrancar desde el USB

Entra en la BIOS/UEFI (tecla `Del`, `F2` o `F12` según el modelo) y pon el USB como primer dispositivo de arranque.

### Opciones de instalación

Selecciona **"Graphical Installation"** y acepta el EULA. Al elegir el disco, haz clic en **"Options"** para seleccionar el sistema de ficheros:

| Sistema de ficheros | Cuándo usarlo |
|---|---|
| `ext4` | Uso general. Opción por defecto recomendada |
| `ZFS (RAID 0)` | Si quieres snapshots nativos y tienes varios discos |

### Configuración regional

| Campo | Ejemplo |
|---|---|
| País | Spain |
| Zona horaria | Europe/Madrid |
| Teclado | Spanish |

### Configuración de red

Usa una **IP fija** fuera del rango DHCP de tu router para evitar conflictos.

| Campo | Ejemplo |
|---|---|
| Hostname | `proxmox.local` |
| IP Address | `192.168.1.100/24` |
| Gateway | `192.168.1.1` |
| DNS Server | `1.1.1.1` |

Tras la instalación, **retira el USB** antes de reiniciar.

---

## Fase 3 — Primer acceso

Desde otro equipo de tu red, abre el navegador y entra en:

```
https://<TU_IP>:8006
```

El navegador mostrará un aviso de certificado autofirmado — es normal. Acepta la excepción de seguridad.

Inicia sesión con usuario `root`, la contraseña que creaste y realm **Linux PAM standard authentication**.

> En la barra superior derecha puedes activar el **modo oscuro** con "Color Theme → Proxmox Dark".

---

## Fase 4 — Post-instalación con Helper Scripts

Esta fase es crítica. Por defecto Proxmox tiene repositorios de pago activos y muestra alertas de suscripción. Los Community Helper Scripts lo solucionan automáticamente.

Abre la **Shell** de tu nodo (panel izquierdo → clic en el nodo → pestaña "Shell") y ejecuta cada script:

### Post-Install (el más importante)

```bash
bash -c "$(curl -fsSL https://raw.githubusercontent.com/community-scripts/ProxmoxVE/main/tools/pve/post-pve-install.sh)"
```

Guía de respuestas recomendadas:

| Pregunta | Respuesta |
|---|---|
| Actualizar paquetes | ✅ YES |
| Deshabilitar repo Enterprise | ✅ YES |
| Habilitar repo No-Subscription | ✅ YES |
| Eliminar mensaje de suscripción | ✅ YES |
| Habilitar Alta Disponibilidad | ❌ NO |
| Habilitar Corosync | ❌ NO |

### CPU Scaling Governor

```bash
bash -c "$(curl -fsSL https://raw.githubusercontent.com/community-scripts/ProxmoxVE/main/tools/pve/scaling-governor.sh)"
```

### Kernel Clean

```bash
bash -c "$(curl -fsSL https://raw.githubusercontent.com/community-scripts/ProxmoxVE/main/tools/pve/kernel-clean.sh)"
```

### Microcode

```bash
bash -c "$(curl -fsSL https://raw.githubusercontent.com/community-scripts/ProxmoxVE/main/tools/pve/microcode.sh)"
```

---

## Fase 5 — Herramientas adicionales

### ProxMenux

Menú interactivo con decenas de utilidades e instalación de servicios con un clic.

```bash
bash -c "$(wget -qLO - https://raw.githubusercontent.com/MacRimi/ProxMenux/main/install_proxmenux.sh)"
```

Una vez instalado, ábrelo escribiendo `menu` en la terminal. Desde aquí puedes instalar AdGuard Home, Pi-hole, Nginx Proxy Manager y mucho más.

### Figurine

Muestra el nombre del servidor en ASCII art al abrir la terminal y crea automáticamente los alias de mantenimiento en `.bashrc`.

> Se instala desde: ProxMenux → Settings → Post Install → Customizable → Figurine

### Clean LXCs

```bash
bash -c "$(curl -fsSL https://raw.githubusercontent.com/community-scripts/ProxmoxVE/main/tools/pve/clean-lxcs.sh)"
```

---

## Alias de mantenimiento en `.bashrc`

Edita el archivo con `nano ~/.bashrc` y añade al final:

```bash
# ─────────────────────────────────────────────
# PROXMOX — ALIAS DE MANTENIMIENTO
# ─────────────────────────────────────────────

alias update='apt update && apt dist-upgrade'
alias aptup='apt update && apt dist-upgrade'

alias clean='curl -fsSL https://raw.githubusercontent.com/community-scripts/ProxmoxVE/main/tools/pve/clean-lxcs.sh | bash'
alias lxcclean='curl -fsSL https://raw.githubusercontent.com/community-scripts/ProxmoxVE/main/tools/pve/clean-lxcs.sh | bash'

alias lxcupdate='curl -fsSL https://raw.githubusercontent.com/community-scripts/ProxmoxVE/main/tools/pve/update-lxcs.sh | bash'
alias kernelclean='curl -fsSL https://raw.githubusercontent.com/community-scripts/ProxmoxVE/main/tools/pve/kernel-clean.sh | bash'
alias cpugov='curl -fsSL https://raw.githubusercontent.com/community-scripts/ProxmoxVE/main/tools/pve/scaling-governor.sh | bash'
alias lxctrim='curl -fsSL https://raw.githubusercontent.com/community-scripts/ProxmoxVE/main/tools/pve/fstrim.sh | bash'
alias updatecerts='pvecm updatecerts'

# ─────────────────────────────────────────────
# BENCHMARKING DE DISCO (requiere fio)
# ─────────────────────────────────────────────

alias seqwrite='sync; fio --randrepeat=1 --ioengine=libaio --direct=1 --name=test --filename=test --bs=4M --size=32G --readwrite=write --ramp_time=4'
alias seqread='sync; fio --randrepeat=1 --ioengine=libaio --direct=1 --name=test --filename=test --bs=4M --size=32G --readwrite=read --ramp_time=4'
alias ranwrite='sync; fio --randrepeat=1 --ioengine=libaio --direct=1 --name=test --filename=test --bs=4k --size=4G --readwrite=randwrite --ramp_time=4'
alias ranread='sync; fio --randrepeat=1 --ioengine=libaio --direct=1 --name=test --filename=test --bs=4k --size=4G --readwrite=randread --ramp_time=4'
```

Recarga la configuración con `source ~/.bashrc`.

### Referencia de alias

| Alias | Acción |
|---|---|
| `update` / `aptup` | Actualiza el sistema completo |
| `clean` / `lxcclean` | Limpia contenedores LXC |
| `lxcupdate` | Actualiza todos los contenedores LXC |
| `kernelclean` | Elimina kernels antiguos |
| `cpugov` | Configura el governor de CPU |
| `lxctrim` | TRIM en contenedores (SSDs) |
| `updatecerts` | Renueva certificados del clúster |
| `seqwrite` / `seqread` | Benchmark secuencial |
| `ranwrite` / `ranread` | Benchmark aleatorio 4K |

---

## Paquetes recomendados

```bash
apt install lm-sensors htop parted fio -y
```

| Paquete | Descripción |
|---|---|
| `lm-sensors` | Monitoriza temperaturas de CPU y discos |
| `htop` | Monitor de procesos interactivo |
| `parted` | Gestión de particiones |
| `fio` | Benchmarking de disco |

Tras instalar `lm-sensors`:

```bash
sensors-detect   # Detecta sensores (responde YES a todo)
sensors          # Muestra temperaturas en tiempo real
```

---

## Benchmarking de disco con fio

Los alias de benchmarking crean un archivo temporal de 4–32 GB. Asegúrate de tener espacio y bórralo después:

```bash
rm test
```

---

## Control desde el móvil

Puedes gestionar Proxmox desde el smartphone con **ProxMon** (iOS/Android) o accediendo directamente al WebUI desde el navegador móvil en `https://<IP>:8006`.

---

## Referencia rápida de mantenimiento

```bash
update        # Actualizar sistema
lxcupdate     # Actualizar todos los contenedores
clean         # Limpiar contenedores
kernelclean   # Eliminar kernels viejos
menu          # Abrir ProxMenux
```

---

## Referencias

| Recurso | URL |
|---|---|
| Web oficial Proxmox | [proxmox.com](https://www.proxmox.com) |
| Community Helper Scripts | [github.com/community-scripts/ProxmoxVE](https://github.com/community-scripts/ProxmoxVE) |
| ProxMenux | [github.com/MacRimi/ProxMenux](https://github.com/MacRimi/ProxMenux) |
| Balena Etcher | [etcher.balena.io](https://etcher.balena.io) |
| Documentación oficial | [pve.proxmox.com/wiki](https://pve.proxmox.com/wiki/Main_Page) |
| Foro oficial | [forum.proxmox.com](https://forum.proxmox.com) |

---

*Hecha con ❤️ para la comunidad homelab*
