# Instalar y Configurar HAProxy en Ubuntu 22.04.2

**Actualizar servidor**

```bash
sudo apt update && sudo apt -y upgrade
```

Para configurar HAProxy en un nuevo servidor con la IP 10.1.4.55 para que funcione con tu clúster MariaDB, sigue estos pasos:

1. **Instalación de HAProxy:** Primero, asegúrate de tener HAProxy instalado en tu servidor. Puedes instalarlo usando el siguiente comando en Ubuntu:

```bash
sudo apt install haproxy

sudo systemctl enable haproxy

sudo systemctl start haproxy

sudo systemctl status haproxy
```

2. **Configuración de HAProxy:** Edita el archivo de configuración de HAProxy `/etc/haproxy/haproxy.cfg` con un editor de texto:

```bash
sudo vim /etc/haproxy/haproxy.cfg
```

3. **Configuración del frontend:** Agrega una sección `frontend` para definir cómo HAProxy debe manejar las conexiones entrantes. Por ejemplo:

```ini
frontend mariadb_frontend
    bind 10.1.4.55:3306
    mode tcp
    default_backend mariadb_backend
```

Este frontend escuchará en la IP 10.1.4.51 en el puerto 3306 (puerto de MySQL) y dirigirá el tráfico al backend `mariadb_backend`.

4. **Configuración del backend:** Agrega una sección `backend` para definir cómo HAProxy debe manejar las conexiones salientes. Por ejemplo:

```ini
backend mariadb_backend
    mode tcp
    balance roundrobin
    server mariadb1 10.1.4.53:3306 check weight 1
    server mariadb2 10.1.4.54:3306 check weight 1
```

Este backend contiene los nodos del clúster MariaDB. `balance roundrobin` distribuirá las conexiones entre los nodos de manera equitativa.

5. **Reinicia HAProxy:** Guarda los cambios en el archivo de configuración y reinicia HAProxy para aplicarlos:

```bash
sudo systemctl restart haproxy
```

6. **Verificación:** Verifica que HAProxy esté funcionando correctamente y que las conexiones al clúster MariaDB se estén distribuyendo correctamente entre los nodos.

Con estos pasos, deberías tener HAProxy configurado para dirigir el tráfico a tu clúster MariaDB con dos nodos en las IPs 10.1.4.53 y 10.1.4.54 desde tu servidor en la IP 10.1.4.55.
