# m08-openssh
## Enunciado
### Configuración de un servidor SSH con certificados
En una máquina nueva, crea un usuario admin. En otra máquina (nueva o no), crea un usuario ubuntu. Logra que desde la máquina del usuario ubuntu se pueda acceder por SSH al usuario admin en la otra máquina sin necesidad de contraseña. Para ello, utiliza certificados autogenerados.
Comprueba que puedes acceder  con los certificados y documenta cómo lo pruebas.
Deshabilita el acceso por contraseña para el usuario admin cuando se utilice el servidor SSH.

Crear usuario admin en servidor:
<pre><code>sudo useradd -m -s /bin/bash admin
sudo passwd admin</code></pre>
Crear usuario ubuntu en cliente:
<pre><code>sudo useradd -m -s /bin/bash prova
sudo passwd prova</code></pre>

### Instalación openssh cliente y servidor

<pre><code>sudo apt install openssh-client
sudo apt install openssh-server</code></pre>

Generar un par de claves rsa, una pública y una privada

<pre><code>ssh-keygen -t rsa</code></pre>

Fichero en que guardar la clave:
<pre><code>/home/prova/.ssh/id_rsa
</code></pre>
NO PONER PASSPHRASE, DEJAR EN BLANCO:
<pre><code>Enter passphrase (empty for no passphrase):

Your identification has been saved in /home/prova/.ssh/id_rsa

Your public key has been saved in /home/prova/.ssh/id_rsa.pub
</code></pre>

Ahora hay que copiar la clave pública al sistema al que nos queremos conectar por SSH sin contraseña:

<pre><code>ssh-copy-id -i id_rsa.pub provaserver@admin</code></pre>







Reiniciar servicio ssh para aplicar los cambios:
<pre><code>sudo systemctl restart sshd.service</code></pre>






















https://ubuntu.com/server/docs/service-openssh
