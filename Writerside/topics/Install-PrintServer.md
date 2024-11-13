# Instalar PrintServer

## Instalación de Apache y Git

1. Instalar ambos paquetes:
    ```bash
    sudo apt-get install apache2
    sudo apt-get install git
- Hasta el momento, las versiones actuales funcionan sin problema.

## Instalación de PHP y Composer


1. Configuramos el repositorio par descargar la version antigua:
   ```bash
   echo "deb http://archive.debian.org/debian buster main" | sudo tee /etc/apt/sources.list.d/buster.list
   sudo apt clean
   sudo apt update

2. El proyecto necesita una versión especifica de PHP:
   ```bash
   sudo apt install php-common=2:69
   sudo apt install php7.3
   sudo apt install php7.3-curl php7.3-gd php7.3-mbstring php7.3-mysql php7.3-xml

2. Instalar Composer:
   ```bash
   mkdir php
   cd php
   php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"
   php composer-setup.php --version=2.0.10
   sudo mv composer.phar /usr/local/bin/composer
   composer -v

- Esto Instalará php 7.3 y Composer 2.0.10

## Configuración de Apache

1. Abrimos el archivo de configuración de apache:
   ```bash
   sudo nano /etc/apache2/apache2.conf

2. Nos aseguramos de que contenga las siguientes configuraciones, normalmente el cambio es en 'AllowOverride'
   ```ini
   <Directory />
    Options FollowSymLinks
    AllowOverride None
    Require all denied
   </Directory>
   
   <Directory /usr/share>
       AllowOverride None
       Require all granted
   </Directory>
   
   <Directory /var/www/>
       Options Indexes FollowSymLinks
       AllowOverride All
       Require all granted
   </Directory>

3. Guarda y cierra el archivo (Ctrl + X, luego Y, y presiona Enter).

## Configuración del Proyecto

1. Navegamoa al directorio /var/www/html/ y luego clona el proyecto (probablemente se necesite una contraseña de aplicación)
   ```bash
   cd /var/www/html/

2. Instalamos las dependencias de Composer:
   ```bash
   cd /var/www/html/raspberry-print-server/
   sudo composer install

3. Habilitamos el módulo rewrite de Apache y reinicia el servicio:
   ```bash
   sudo a2enmod rewrite
   sudo systemctl restart apache2

4. Instalamos las libreras de Python faltantes (python-escpos):
   ```bash
   sudo -H python3 -m pip install python-escpos --break-system-packages

5. Instalamos las libreras de Python faltantes (pytz):
   ```bash
   sudo -H python3 -m pip install pytz --break-system-packages

6. Preferencia personal, reiniciamos el sistema:
   ```bash
   sudo reboot