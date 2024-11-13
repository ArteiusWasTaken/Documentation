# Instalación y Configuración de No-IP en Raspberry Pi

## Preparación

1. Descarga y extrae el cliente de No-IP:
    ```bash
    cd /usr/local/src/
    sudo wget http://www.no-ip.com/client/linux/noip-duc-linux.tar.gz
    sudo tar xf noip-duc-linux.tar.gz
    cd noip-2.1.9-1/

2. Compila e instala el cliente:
    ```bash
    sudo make
    sudo make install

- Aqui pedirá las credenciales para conectarse a la cuenta de no-ip, después preguntará por que dirección es la que
  vamos a actualizar en el Raspberry y al final hay que configurar el intervalo y scripts como sigue:

    * Interval: 5
    * Run scripts on end? Y/n: n

3. Generamos el archivo de configuración:
    ```bash
    sudo /usr/local/bin/noip2 -C

- Aqui volveremos a configurar todo como en el paso 2.

## Configuración del Servicio

1. Edita el archivo de servicio:
    ```bash
    sudo nano /etc/systemd/system/noip2.service

2. Copia y pega el siguiente contenido:
    ```ini
    # Simple No-ip.com Dynamic DNS Updater
    #
    # By Nathan Giesbrecht (http://nathangiesbrecht.com)
    #
    # 1) Install binary as described in no-ip.com's source file (assuming results in /usr/local/bin)
    # 2) Run sudo /usr/local/bin/noip2 -C to generate configuration file
    # 3) Copy this file noip2.service to /etc/systemd/system/
    # 4) Execute sudo systemctl enable noip2
    # 5) Execute sudo systemctl start noip2
    #
    # systemd supports lots of fancy features, look here (and linked docs) for a full list:
    #   http://www.freedesktop.org/software/systemd/man/systemd.exec.html
    
    [Unit]
    Description=No-ip.com dynamic IP address updater
    After=network.target
    After=syslog.target
    
    [Install]
    WantedBy=multi-user.target
    Alias=noip.service
    
    [Service]
    # Start main service
    ExecStart=/usr/local/bin/noip2
    Restart=always
    Type=forking


4. Recarga los demonios de sistema y habilita el servicio de No-IP:
    ```bash
   sudo systemctl daemon-reload
   sudo systemctl enable noip2
   sudo systemctl start noip2


5. Verifica el estado del servicio:
    ```bash
    sudo systemctl status noip2

6. Preferencia personal, reiniciamos el sistema:
   ```bash
   sudo reboot
