# Guía de Instalación de Cockpit y Verificación de Puertos (MediaMTX)

Este documento detalla los pasos para instalar y configurar **Cockpit** en servidores Linux (Ubuntu/Debian) y verificar la coexistencia de puertos cuando se ejecuta junto a servicios como **MediaMTX**.

---

## 📋 Requisitos Previos

- **Sistema Operativo:** Ubuntu 20.04 / 22.04 / 24.04 LTS o Debian 11 / 12.
- **Usuario:** Privilegios de superusuario (`sudo`).
- **Acceso:** Terminal SSH / consola local.

---

## 🚀 1. Instalación de Cockpit

### Paso 1: Actualizar el sistema e instalar Cockpit
```bash
sudo apt update && sudo apt install -y cockpit

Paso 2: Habilitar e iniciar el socket del servicio
Cockpit utiliza socket activation administrado por systemd:

sudo systemctl enable --now cockpit.socket

Paso 3: Configurar el Firewall (UFW)
Permite el tráfico a través del puerto por defecto (9090/tcp):

sudo ufw allow 9090/tcp

4. Acceso a la Interfaz Web
Abre un navegador web e ingresa la IP de tu servidor:

https://<IP_DEL_SERVIDOR>:9090

Si recibes una advertencia de certificado de seguridad no válido, acéptala para continuar. Cockpit genera un certificado SSL autofirmado por defecto

