# Instalacion y Configuracion Ubuntu

### **`Ubuntu 22.04`**

Verificamos Version

```bash
cat /etc/*release
```

1. Verificar con si existe ifconfig, si no instalamos:

```bash
sudo apt update

sudo apt install net-tools
```

2. Establecer Zona Horaria Zona horaria:

```bash
sudo timedatectl set-timezone America/Mexico_City
```

3. Verificamos Fecha:

```bash
date
```

4. Instalamos

```bash
sudo apt-get install openvswitch-switch-dpdk
```

5. Establecemos permisos al archivo de red

```bash
sudo chmod 600 /etc/netplan/00-installer-config.yaml
```

6. Configuramos el archivo de red:

```bash
sudo vim /etc/netplan/00-installer-config.yaml
```

```yaml
network:
  ethernets:
    enp0s3:
      dhcp4: false
      addresses:
        - 10.1.4.249/24
      routes:
        - to: default
          via: 10.1.4.1
      nameservers:
        addresses:
          - 8.8.8.8
          - 8.8.4.4
  version: 2
```

7. Asignamos IP Estatica:

```bash
sudo netplan apply
```

### Habilitar conexiones SSH

Para configurar su servidor de modo que permita las conexiones SSH entrantes, puede utilizar este comando:

```bash
 sudo ufw allow ssh
```

Esto creará reglas de firewall que permitirán todas las conexiones en el puerto 22, que es el que escucha el demonio SSH por defecto.

Tambien podemos escribir la regla equivalente especificando el puerto en vez del nombre del servicio.
Por ejemplo, este comando funciona como el anterior:

```bash
sudo ufw allow 22
```

Si configuró su demonio SSH para utilizar un puerto diferente, deberá especificar el puerto apropiado. Por ejemplo, si su servidor SSH escucha en el puerto 2222 puede utilizar este comando para permitir las conexiones en ese puerto:

```bash
sudo ufw allow 2222
```

Ahora que su firewall está configurado para permitir las conexiones SSH entrantes, podemos habilitarlo.

Paso 4: Habilitar UFW
Para habilitar UFW, utilice este comando:

```bash
sudo ufw enable
```

Recibirá una advertencia que indicará que el comando puede interrumpir las conexiones SSH existentes. Ya configuramos una regla de firewall que permite conexiones SSH. Debería ser posible continuar sin inconvenientes. Responda a la solicitud con y y presione ENTER.

Ejecute el comando

```bash
sudo ufw status verbose
```
