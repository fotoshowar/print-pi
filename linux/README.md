# 🖨️ FotoShow Print Server — Linux / Raspberry Pi

## Instalación rápida

```bash
# 1. Clonar
git clone https://github.com/fotoshowar/print-server.git
cd print-server

# 2. Instalar (como root)
sudo bash linux/install.sh

# 3. Configurar
sudo nano /opt/fotoshow-print/.env

# 4. Listo! Ya está corriendo
```

## Configurar impresora

```bash
# Ver impresoras disponibles
lpstat -p

# Agregar impresora USB (EPSON L805)
# Opción 1: Via CUPS Web (recomendado)
# Abrir http://IP-RASPBERRY:631 en el navegador

# Opción 2: Comando
sudo lpadmin -p EPSON_L805 -E -v usb://EPSON/L805 -m everywhere

# Setear como default
sudo lpoptions -d EPSON_L805

# Editar .env con el nombre de la impresora
sudo nano /opt/fotoshow-print/.env
# DEFAULT_PRINTER=EPSON_L805
```

## Comandos

```bash
# Estado
sudo systemctl status fotoshow-print

# Reiniciar
sudo systemctl restart fotoshow-print

# Logs
sudo journalctl -u fotoshow-print -f

# Parar
sudo systemctl stop fotoshow-print
```

## Túnel SSH (acceso internet)

```bash
# 1. Configurar SSH keys (una sola vez)
sudo ssh-keygen -t ed25519 -f /root/.ssh/fotoshow_key -N ""
sudo ssh-copy-id -i /root/.ssh/fotoshow_key root@207.148.15.8

# 2. Activar túnel
sudo systemctl enable --now fotoshow-tunnel

# 3. Tu app queda en https://descarga.fotoshow.online
```

## Raspberry Pi como hotspot WiFi

```bash
# Instalar hostapd + dnsmasq
sudo apt install hostapd dnsmasq

# Configurar en /etc/hostapd/hostapd.conf:
# ssid=FotoShow
# wpa_passphrase=fotos2026

# Los clientes se conectan al WiFi "FotoShow"
# y acceden a http://192.168.4.1:3000
```

## Estructura

```
/opt/fotoshow-print/
├── server-linux.js     # Servidor (CUPS)
├── public/index.html   # Web app
├── uploads/            # Fotos originales por día
├── thumbs/             # Thumbnails por día
├── db.json             # Base de datos
└── .env                # Configuración
```
