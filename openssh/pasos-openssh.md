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

<pre><code>root@prova-VirtualBox:~# ssh-keygen -t rsa

Generating public/private rsa key pair.
</code></pre>

Fichero en que guardar la clave:

<pre><code>Enter file in which to save the key (/root/.ssh/id_rsa): /root/.ssh/my_id

Created directory '/root/.ssh'.</code></pre>

NO PONER PASSPHRASE, DEJAR EN BLANCO:

<pre><code>Enter passphrase (empty for no passphrase): 

Enter same passphrase again: 

Your identification has been saved in /root/.ssh/my_id

Your public key has been saved in /root/.ssh/my_id.pub

The key fingerprint is:

SHA256:O67S0+56Et5KJJI78pXlkSuH/ohPQBJJodZnj7xkwOk root@prova-VirtualBox

The key's randomart image is:

+---[RSA 3072]----+

|o+.              |

|o.o .            |

|o..= o           |

|.o..= o.         |

|  +E.==.S        |

|   +oBoo .       |

|. o *+=+o        |

| o *.==.+.       |

|  o.+o=X+        |

+----[SHA256]-----+

</code></pre>

Vamos al directorio donde están las claves:

<pre><code>root@prova-VirtualBox:~# cd /root/.ssh/

root@prova-VirtualBox:~/.ssh# ls

my_id  my_id.pub
</code></pre>

En el servidor:
nano /etc/ssh/sshd

Añadir estas líneas:
<pre><code>RSAAuthentication yes
PubkeyAuthentication yes</code></pre>

En cliente y servidor:
<pre><code>cd /root
chmod 700 .ssh</code></pre>

añadir máquina virtual a hosts
<pre><code>nano /etc/hosts
GNU nano 6.2                       /etc/hosts *                               

127.0.0.1       localhost

127.0.1.1       prova-VirtualBox



# The following lines are desirable for IPv6 capable hosts

::1     ip6-localhost ip6-loopback

fe00::0 ip6-localnet

ff00::0 ip6-mcastprefix

ff02::1 ip6-allnodes

ff02::2 ip6-allrouters



192.168.0.31 provaserver
</code></pre>

Permitir puerto 22 para el cortafuegos
<pre><code>apt install firewalld
firewall-cmd --permanent --add-port=22/tcp
firewall-cmd --reload
sudo ufw allow 22/tcp
sudo ufw reload</code></pre>

Ahora hay que copiar la clave pública al sistema al que nos queremos conectar por SSH sin contraseña:

<pre><code>cd /root/.ssh
ssh-copy-id -i my_id.pub root@provaserver</code></pre>

Reiniciar servicio ssh para aplicar los cambios:
<pre><code>sudo systemctl restart sshd.service
ssh root@192.168.0.31
</code></pre>






















https://toolspond.com/fix-ssh-could-not-resolve-hostname/
https://ubuntu.com/server/docs/service-openssh
https://www.ibm.com/support/pages/configuring-ssh-login-without-password
https://www.tecmint.com/fix-no-route-to-host-ssh-error-in-linux/

