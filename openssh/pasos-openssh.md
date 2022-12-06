# m08-openssh
## Enunciado
### Configuración de un servidor SSH con certificados
En una máquina nueva, crea un usuario admin. En otra máquina (nueva o no), crea un usuario ubuntu. Logra que desde la máquina del usuario ubuntu se pueda acceder por SSH al usuario admin en la otra máquina sin necesidad de contraseña. Para ello, utiliza certificados autogenerados.
Comprueba que puedes acceder  con los certificados y documenta cómo lo pruebas.
Deshabilita el acceso por contraseña para el usuario admin cuando se utilice el servidor SSH.

Crear usuario admin:
<pre><code>sudo useradd -m -s /bin/bash admin
sudo passwd admin</code></pre>
Crear usuario ubuntu:
<pre><code>sudo useradd -m -s /bin/bash usu1
sudo passwd usu1</code></pre>

### Instalación openssh cliente y servidor

<pre><code>sudo apt install openssh-client</code></pre>
<pre><code>sudo apt install openssh-server</code></pre>






















https://ubuntu.com/server/docs/service-openssh
