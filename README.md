# Navigation of a mobile robot on sidewalks
Autonomous navigation system for a basic differential robot for navigation on sidewalks, using the ROS2 navigation stack and ROS2 foxy distribution.

![image](https://user-images.githubusercontent.com/81580183/170148198-a42197aa-04b3-431e-8105-76b4a02bf8ac.png)

## Descarga del proyecto
Basta con hacer un git clone o descargando el .zip.
```bash
git clone https://github.com/JuanGuzman-ai/parcialFinal-StreamaFirewall.git
```

## Instalación Streama en la maquina serverStreama

Actualizamos la maquina e instalamos las librerias necesarias

```bash
sudo -i
yum update -y
yum install vim wget httpd mod_ssl -y
```
Instalamos Java 8 JDK 
```bash
yum install java-1.8.0-openjdk-devel
```
Descargamos el war de Streama
```bash
wget https://github.com/streamaserver/streama/releases/download/v1.6.1/streama-1.6.1.war
```
Creamos la carpeta streama y movemos el .war descargado
```bash
mkdir /opt/streama
mv streama-1.6.1.war /opt/streama/streama.war
```
Creamos la carpeta media en el directorio creado anterior y le damos privilegios
```bash
mkdir /opt/streama/media
chmod 664 /opt/streama/media
chmod 777 /etc/systemd/system
```
Entramos a editar el servicio de streama
```bash
vim /etc/systemd/system/streama.service 
```
Y agregamos las siguiente líneas
```bash
[Unit]
Description=Streama Server
After=syslog.target
After=network.target

[Service]
User=root
Type=simple
ExecStart=/bin/java -jar /opt/streama/streama.war
Restart=always
StandardOutput=syslog
StandardError=syslog
SyslogIdentifier=Streama

[Install]
WantedBy=multi-user.target
```

Dejamos funcionando los servicios de Streama y httpd
```bash
systemctl start streama
systemctl enable streama
systemctl start httpd
systemctl enable httpd
```
## Preparación del Firewall en la maquina serverFirewall
Actualizamos la maquina e instalamos vim
```bash
sudo -i
yum update -y
yum install vim -y
```

Detenemos y apagamos el NetworkManager
```bash
service NetworkManager stop
chkconfig NetworkManager off
```
Configuramos la ip_forward
```bash
vim /etc/sysctl.conf
#Añadimos la siguiente línea al final del archivo
net.ipv4.ip_forward = 1
```

Iniciamos el firewall
```bash
systemctl start firewalld
systemctl enable firewalld
```
Gestionamos las zonas
```bash
firewall-cmd --permanent --zone=dmz --add-interface=eth1
firewall-cmd --permanent --zone=internal --add-interface=eth2
firewall-cmd --reload
```
Agregamos las reglas
```bash
firewall-cmd --direct --permanent --add-rule ipv4 nat POSTROUTING 0 -o eth1 -j MASQUERADE
firewall-cmd --direct --permanent --add-rule ipv4 filter FORWARD 0 -i eth2 -o eth1 -j ACCEPT
firewall-cmd --direct --permanent --add-rule ipv4 filter FORWARD 0 -i eth1 -o eth2 -m state --state RELATED,ESTABLISHED -j ACCEPT
```
Agregamos servicio http a las zona dmz y el puerto
```bash
firewall-cmd --permanent --zone=dmz --add-service=http
firewall-cmd --zone=dmz --add-port=8080/tcp --permanent    
```                                                                                                                                                        
Añadimos el redirect al servidor streama apuntando a la ip del serverStreama
```bash
firewall-cmd --zone="dmz" --add-forward- port=8080:proto=tcp:toport=8080:toaddr=192.168.50.4 --permanent
firewall-cmd --zone="internal" --add-forward- port=8080:proto=tcp:toport=8080:toaddr=192.168.50.4 --permanent
firewall-cmd --reload  
```
## Realizado por
Juan David Delgado Guzmán

Juan Manuel Molica Caicedo
