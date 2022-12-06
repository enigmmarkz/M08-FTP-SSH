# m08-openssh
## Enunciado
### Configuración de un servidor SSH con certificados
En una máquina nueva, crea un usuario admin. En otra máquina (nueva o no), crea un usuario ubuntu. Logra que desde la máquina del usuario ubuntu se pueda acceder por SSH al usuario admin en la otra máquina sin necesidad de contraseña. Para ello, utiliza certificados autogenerados.
Comprueba que puedes acceder  con los certificados y documenta cómo lo pruebas.
Deshabilita el acceso por contraseña para el usuario admin cuando se utilice el servidor SSH.

Crear usuario admin en linux servidor:
<pre><code>sudo useradd -m -s /bin/bash admin
sudo passwd admin</code></pre>
Crear usuario ubuntu en cliente:
<pre><code>sudo useradd -m -s /bin/bash prova
sudo passwd prova</code></pre>

### Instalación openssh

<pre><code>sudo apt-get install ssh</code></pre>

### Conectarnos por ssh entre cliente y servidor
Desde el cliente:
<pre><code>ssh -l admin 192.168.0.31</code></pre>

Ahora tenemos acceso al servidor, y para configurar ssh debemos acceder a los archivos `/etc/ssh/ssh_config` o `/etc/ssh/sshd_config`

### Generar un par de claves rsa, una pública y una privada para poder acceder sin contraseña:

<pre><code>admin@provaserver:~$ ssh-keygen -t rsa
Generating public/private rsa key pair.
</code></pre>

Fichero en que guardar la clave:
<pre><code>Enter file in which to save the key (/home/admin/.ssh/id_rsa): 
Created directory '/home/admin/.ssh'</code></pre>

NO PONER PASSPHRASE, DEJAR EN BLANCO:
<pre><code>Enter passphrase (empty for no passphrase): 
Enter same passphrase again: 
Your identification has been saved in /home/admin/.ssh/id_rsa
Your public key has been saved in /home/admin/.ssh/id_rsa.pub
The key fingerprint is:
SHA256:wPUzBl+dQWbmpDbUzA19Mp59fPlPS95vNjE9+L3FqOs admin@provaserver
The key's randomart image is:
+---[RSA 3072]----+
|        o   o*O* |
|     . . + o BO +|
|      o   * +..*o|
|       . . + .o.=|
|        S     . =|
|             . B+|
|              =.@|
|             . =B|
|           .E. +=|
+----[SHA256]-----+
</code></pre>

Con esto hemos obtenido una clave privada en:
<pre><code>/home/admin/.ssh/id_rsa</code></pre>
Y una clave pública en:
<pre><code>/home/admin/.ssh/id_rsa.pub</code></pre>
Ahora hay que copiar la clave pública al sistema al que nos queremos conectar por SSH sin contraseña en el fichero `authorized_keys`:
<pre><code>cd /home/admin/.ssh
ssh-copy-id admin@192.168.0.31</code></pre>
<pre><code>admin@provaserver:~/.ssh$ ssh admin@192.168.0.31</code></pre>

 Para deshabilitar el acceso por contraseña para el usuario admin cuando se utilice el servidor SSH, y permitir el acceso solamente con las claves SSH que han sido generadas hay que acceder al fichero de configuracion de SSH

En este caso editamos el fichero `sshd_config`:
<pre><code>sudo nano /etc/ssh/sshd_config</code></pre>
Descomentamos la linea `PasswordAuthentication yes` y la editamos dejandola asi:
<pre><code>PasswordAuthentication no
Además descomentamos la linea `PubkeyAuthentication yes` y la línea `AuthorizedKeysFile .ssh/authorized_keys .ssh/authorized_keys2`</code></pre>
<pre><code>sudo chmod 700 .ssh
sudo chmod 600 .ssh/authorized_keys</code></pre>
Reiniciar SSH para aplicar las configuraciones:
<pre><code>sudo systemctl restart ssh
sudo systemctl restart sshd</code></pre>

https://toolspond.com/fix-ssh-could-not-resolve-hostname/
https://ubuntu.com/server/docs/service-openssh
https://www.ibm.com/support/pages/configuring-ssh-login-without-password
https://www.tecmint.com/fix-no-route-to-host-ssh-error-in-linux/
https://www.howtouselinux.com/post/setup-ssh-keys-to-login-linux-without-password


