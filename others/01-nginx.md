# Instalacion y Configuracion NGINX

### **`NGINX`**

1. Instalamos:

```bash
sudo apt install -y nginx
```

2. Aplicar ajustes al firewall:

```bash
sudo ufw app list
```

Debemos obtener:

```
Nginx Full
Nginx HTTP
Nginx HTTPS
OpenSSH
```

3. Activamos:

```bash
sudo ufw allow 'Nginx HTTP'

sudo ufw status
```

4. Comprobar su servidor web:

```bash
sudo systemctl status nginx
```

### Comando de acción

```bash
sudo systemctl stop nginx

sudo systemctl start nginx

sudo systemctl restart nginx

sudo systemctl reload nginx

sudo systemctl disable nginx

sudo systemctl enable nginx
```
