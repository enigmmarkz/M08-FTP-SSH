# m08-ftp
## Enunciado

Crea un repositorio privado en GitHub
Para esta práctica, como en la práctica anterior, entregarás tu trabajo mediante GitHub. Crea un repositorio privado, con dos carpetas:

Una carpeta llamada ProFTPd, con la primera parte de la práctica.
Una carpeta llamada OpenSSH, con la segunda parte de la práctica.
Para cada parte de la práctica, documenta todos los pasos que sigues en un fichero de markdown que subirás en cada una de las carpetas correspondiente, junto con todos los ficheros que hayas modificado y/o generado. NO SUBAS a GitHub claves privadas cuando tengas que generarlas.
Añádeme como colaborador de tu repositorio (pablo-molins).
Sube como respuesta a esta entrega la URL de tu repo.

## Servidor ProFTPd
Crea una máquina virtual en la que configures adecuadamente un servidor ProFTPd y que cumpla con los siguientes requisitos:

Configura un usuario llamado "admin" con contraseña "admin" que pueda iniciar sesión en el servidor FTP vía Filezilla (NUNCA PONGAS ESTA CONTRASEÑA EN UN SERVIDOR REAL).
Crea dos usuarios user1 y user2, con mismo nombre de usuario que contraseña.
Los usuarios no deben poder subir de su directorio personal (cada uno el suyo).
Configura el rango de puertos para el modo pasivo del 62000 al 63000.
Permite un máximo de 30 conexiones simultáneas, 5 de las cuales pueden ser anónimas.
Configura al usuario anónimo para que pueda descargar archivos existentes en el directorio /srv/public del servidor (debe configurar el directorio también).
Añade un certificado autofirmado siguiendo las indicaciones del manual que encontrará en el aula Moodle.

## Explicaciones y comandos

Primero que nada, asegurarse de que todos los paquetes del sistema están actualizados:
<pre><code>sudo apt-get update
sudo apt-get upgrade</code></pre>

### Instalación de proftpd y openssl:

<pre><code>sudo apt-get install -y proftpd openssl</code></pre>
Durante la instalación nos preguntará sobre el tipo de instalación que queremos.
En caso de hacer poco uso del servidor FTP podemos seleccionar la opción «inetd» para ahorrar recursos del sistema.
Si vamos a hacer un uso considerable del servidor FTP con varios usuarios y bastante tiempo deberíamos instalarlo como «standalone» o «independientemente».
Esta configuración se podría modificar posteriormente, pero es recomendable seleccionar «standalone» porque se ejecutará como un servidor independiente, y en caso de tener un alto tráfico, funcionará mejor.


### Configuración de proftpd:

Editamos el siguiente fichero:
<pre><code>sudo nano /etc/proftpd/proftpd.conf</code></pre>

Y en su contenido lo configuramos de esta manera:

- ServerName: "provaserver"
- DefaultRoot ~ (ENTRE defaultroot y VIRULILLA (~)HAY QUE PONER UN ESPACIO PARA QUE NO DÉ ERROR) descomentar esta linea para restringir el acceso de los usuarios a carpetas fuera de su carpeta de usuario.
- PassivePorts 62000 63000 (para configurar el rango de puertos para el modo pasivo)
- Descomentar línea: #Include /etc/proftpd/tls.conf
- Descomentar todo este contenido y que quede así:

```
# <Anonymous ~ftp>
#   User ftp
#   Group nogroup
#   # We want clients to be able to login with "anonymous" as well as "ftp"
#   UserAlias anonymous ftp
#   # Cosmetic changes, all files belongs to ftp user
#   #DirFakeUser on ftp
#   #DirFakeGroup on ftp
# 
#   RequireValidShell off
# 
#   # Limit the maximum number of anonymous logins
#   MaxClients 5
# 
#   # We want 'welcome.msg' displayed at login, and '.message' displayed
#   # in each newly chdired directory.
#   DisplayLogin welcome.msg
#   DisplayChdir .message
# 
#   # Limit WRITE everywhere in the anonymous chroot
#   <Directory /srv/public>
#     <Limit ALL>
#       AllowAll
#     </Limit>
#   </Directory>
# 
#   # Uncomment this if you're brave.
#   # <Directory incoming>
#   #   # Umask 022 is a good standard umask to prevent new files and dirs
#   #   # (second parm) from being group and world writable.
#   #   Umask022  022
#   #   <Limit READ WRITE>
#   #     DenyAll
#   #     </Limit>
#   #       <Limit STOR>
#   #         AllowAll
#   #     </Limit>
#   # </Directory>
# 
# </Anonymous> </code></pre>
```
(para cambiar el máximo de conexiones simultáneas anónimas a 5, establecer: MaxClients 5 )
```
 # Limit the maximum number of anonymous logins
   MaxClients 5
```
(para configurar al usuario anónimo para que pueda descargar archivos existentes en el directorio /srv/public establecer: 
```
<Directory /srv/public>
     <Limit ALL>
       AllowAll
     </Limit>
   </Directory>)
```

dejar comentado así:
```
   #DirFakeUser on ftp
   #DirFakeGroup on ftp
```
- Editar fichero tls.conf de /etc/proftpd/tls.conf y dejar las siguientes líneas descomentadas como se muestra:
```
<IfModule mod_tls.c>
TLSEngine                               on
TLSLog                                  /var/log/proftpd/tls.log
TLSProtocol                             SSLv23
 
TLSRSACertificateFile                   /etc/ssl/certs/proftpd.crt
TLSRSACertificateKeyFile                /etc/ssl/private/proftpd.key

#TLSCACertificateFile                                     /etc/ssl/certs/CA.pem
TLSOptions                      NoCertRequest EnableDiags NoSessionReuseRequired
TLSVerifyClient                         off
TLSRequired                             on
TLSRenegotiate                          required on
</IfModule>
```

Reiniciar servicio proftpd para aplicar los cambios:

<pre><code>sudo service proftpd restart</code></pre>

Crear usuario admin y su contraseña admin:
<pre><code>sudo useradd -m -s /bin/bash admin
sudo passwd admin</code></pre>
Ahora desde FileZilla nos conectamos con los datos (en mi caso):

- Servidor: sftp://192.168.141.12
- Nombre de usuario: admin
- Contraseña: admin
- Puerto 22

Crear usuarios user1 y user2 con mismo usuario y contraseña:
<pre><code>sudo useradd -m -s /bin/bash user1
sudo passwd user1</code></pre>
<pre><code>sudo useradd -m -s /bin/bash user2
sudo passwd user2</code></pre>

Configurar tls con proftpd creando un certificado SSL:
<pre><code>sudo openssl req -x509 -newkey rsa:2048 -keyout /etc/ssl/private/proftpd.key -out /etc/ssl/certs/proftpd.crt -nodes -days 365</code></pre>
Cambiar permisos del archivo del certificado:
<pre><code>sudo chmod 600 /etc/ssl/certs/proftpd.crt
sudo chmod 600 /etc/ssl/private/proftpd.key</code></pre>


https://vitux.com/ubuntu-proftpd-tls/
