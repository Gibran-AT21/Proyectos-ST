# 💾  Configuración del Servidor y Herramientas para VPS

## 🖥️ Configuración del Dominio en Hostinger

Para utilizar un dominio personalizado en Hostinger y vincularlo con Hestia Control Panel, sigue estos pasos:

### 📌 1️⃣ Registro y Configuración del Dominio
1. Accede a tu cuenta de [Hostinger](https://www.hostinger.com/).
2. Dirígete a la sección **Dominios** y selecciona **Registrar un nuevo dominio**.
3. Introduce el nombre del dominio que deseas adquirir (por ejemplo: `tudominio.com`).
4. Completa el pago y activa tu dominio (el proceso puede tardar hasta 24 horas).

### 🌐 2️⃣ Configuración de Servidores de Nombres (DNS)
1. En el panel de Hostinger, ve a **Dominios** > **Administrar**.
2. En la sección **Nameservers (DNS)**, elige la opción de **Usar servidores de nombres personalizados**.
3. Configura los siguientes registros DNS apuntando a tu servidor en Oracle Cloud:
   - `ns1.tudominio.com`
   - `ns2.tudominio.com`
4. Guarda los cambios y espera la propagación del DNS (puede tomar algunas horas).

---

## ⚙️ 3️⃣ Vinculación del Dominio con Hestia Control Panel

### 📌 Acceder a la VPS
1. Conéctate a tu servidor a través de SSH con el siguiente comando:
   ```sh
   ssh root@<IP_DEL_SERVIDOR>
   ```

### 🛠️ Configuración en Hestia
1. Accede a **Hestia Control Panel** y sigue los pasos para configurar tu entorno.
2. Se generará un comando SSH que debes ejecutar en tu servidor con permisos de root para instalar Hestia.

### 🔗 Instalación de Hestia
Ejecuta el comando de instalación de Hestia en tu terminal SSH:
   ```sh
   cd /usr/local/hestia/bin/
   v-change-sys-hostname tudominio.com
   v-add-letsencrypt-host
   ```

---

## 🔍 Verificación de Propagación de DNS
Para comprobar que los cambios de DNS se han propagado correctamente:
1. Accede a una herramienta de verificación de DNS como [DNS Propagator](https://dnschecker.org/).
2. Introduce tu dominio (`tudominio.com`).
3. Verifica los registros DNS.

✅ **Si la propagación es exitosa**, el dominio estará listo para usarse.

---

## 🌎 Instalación de Apache y PHP en Oracle Cloud

### 📌 Conexión a la Instancia
1. Inicia sesión en [Oracle Cloud Console](https://cloud.oracle.com/).
2. Ve a **Compute** > **Instances** y selecciona la instancia creada.
3. Obtén la dirección IP pública y conéctate por SSH:
   ```sh
   ssh -i <tu-clave-privada.pem> ubuntu@<IP_DE_LA_INSTANCIA>
   ```

### 📡 Instalación de Apache
Ejecuta los siguientes comandos para instalar Apache:
   ```sh
   sudo apt update
   sudo apt -y install apache2
   ```

Para verificar que Apache está funcionando, accede a la IP pública desde un navegador.

### 🔥 Configuración del Firewall
Habilita el tráfico web en el puerto 80:
   ```sh
   sudo iptables -I INPUT 6 -m state --state NEW -p tcp --dport 80 -j ACCEPT
   sudo netfilter-persistent save
   ```

### ⚙️ Prueba de PHP
Crea un archivo de prueba en `/var/www/html/`:
   ```sh
   echo "<?php phpinfo(); ?>" | sudo tee /var/www/html/info.php
   ```
Accede a `http://<IP_DEL_SERVIDOR>/info.php` para verificar la configuración.

---

## 🔐 Configuración de HTTPS con Let’s Encrypt

### 📌 Activar SSL en Hestia
1. Accede a **Hestia Control Panel** desde `https://<IP_DEL_SERVIDOR>:8083/`.
2. Ve a **Web** > **Añadir Dominio**.
3. Ingresa tu dominio (`tudominio.com`) y marca:
   - **Habilitar Soporte SSL**
   - **Usar Let’s Encrypt**
4. Guarda los cambios.

### 🛠️ Instalación Manual de SSL
Si Let’s Encrypt no se activa automáticamente, puedes ejecutarlo manualmente por SSH:
   ```sh
   v-add-web-domain-ssl usuario_hestia tudominio.com
   v-add-letsencrypt-domain usuario_hestia tudominio.com
   ```

✅ **Con esto, tu sitio web estará protegido con HTTPS.** 🚀

