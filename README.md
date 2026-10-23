Guía de Instalación de Cockpit y Verificación de Puertos (MediaMTX)Este documento detalla los pasos para instalar y configurar Cockpit en servidores Linux (Ubuntu/Debian) y verificar la coexistencia de puertos cuando se ejecuta junto a servicios como MediaMTX.📋 Requisitos PreviosSistema Operativo: Ubuntu 20.04/22.04/24.04 LTS o Debian 11/12.Usuario con privilegios de superusuario (sudo).Acceso a la terminal.🚀 1. Instalación de CockpitPaso 1: Actualizar el sistema e instalar Cockpitsudo apt update && sudo apt install -y cockpit
Paso 2: Habilitar e iniciar el socket del servicioCockpit utiliza socket activation administrado por systemd:sudo systemctl enable --now cockpit.socket
Paso 3: Configurar el Firewall (UFW)Permite el tráfico a través del puerto por defecto (9090/tcp):sudo ufw allow 9090/tcp
📊 2. Matriz de Puertos (Cockpit vs. MediaMTX)ServicioProtocoloPuerto Por DefectoFunciónCockpitTCP9090Interfaz web de administraciónMediaMTXTCP/UDP8554Servidor RTSPMediaMTXTCP1935Servidor RTMPMediaMTXTCP8888Servidor HLS / WebUIMediaMTXTCP8889Servidor WebRTCMediaMTXUDP8890Servidor SRTMediaMTXTCP9997API y Métricas[!NOTE]
No existen conflictos directos entre Cockpit y MediaMTX en sus configuraciones predeterminadas.🔍 3. Verificación de Servicios y PuertosPara confirmar que ambos servicios escuchan en sus respectivos puertos sin colisiones, ejecuta:sudo ss -tulpn | grep -E '9090|8554|1935|8888|8889|8890|9997'
Ejemplo de salida esperada:udp   UNCONN 0      0             *:8890          *:*    users:(("mediamtx",pid=408144,fd=34))
tcp   LISTEN 0      4096          *:8888          *:*    users:(("mediamtx",pid=408144,fd=18))
tcp   LISTEN 0      4096          *:8889          *:*    users:(("mediamtx",pid=408144,fd=27))
tcp   LISTEN 0      4096          *:9090          *:*    users:(("systemd",pid=1,fd=123))
tcp   LISTEN 0      4096          *:8554          *:*    users:(("mediamtx",pid=408144,fd=10))
tcp   LISTEN 0      4096          *:9997          *:*    users:(("mediamtx",pid=408144,fd=43))
tcp   LISTEN 0      4096          *:1935          *:*    users:(("mediamtx",pid=408144,fd=11))
systemd en el puerto 9090: Es el comportamiento normal de Cockpit (cockpit.socket).mediamtx en los demás puertos: Indica que el servidor de streaming está operando correctamente.🌐 4. Acceso a la Interfaz WebAbre un navegador web e ingresa la siguiente dirección IP:https://<IP_DEL_SERVIDOR>:9090
[!TIP]
Si recibes una advertencia de certificado de seguridad no válido, acéptala para continuar. Cockpit genera un certificado SSL autofirmado por defecto.
