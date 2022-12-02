# m08dns
## Enunciado

Crea un repositorio privado en GitHub
Para esta práctica, como en la práctica anterior, entregarás tu trabajo mediante GitHub. Crea un repositorio privado, con dos carpetas:

Una carpeta llamada ProFTPd, con la primera parte de la práctica.
Una carpeta llamada OpenSSH, con la segunda parte de la práctica.
Para cada parte de la práctica, documenta todos los pasos que sigues en un fichero de markdown que subirás en cada una de las carpetas correspondiente, junto con todos los ficheros que hayas modificado y/o generado. NO SUBAS a GitHub claves privadas cuando tengas que generarlas.
Añádeme como colaborador de tu repositorio (pablo-molins).
Sube como respuesta a esta entrega la URL de tu repo.
Servidor ProFTPd
Crea una máquina virtual en la que configures adecuadamente un servidor ProFTPd y que cumpla con los siguientes requisitos:

Configura un usuario llamado "admin" con contraseña "admin" que pueda iniciar sesión en el servidor FTP vía Filezilla (NUNCA PONGAS ESTA CONTRASEÑA EN UN SERVIDOR REAL).
Crea dos usuarios user1 y user2, con mismo nombre de usuario que contraseña.
Los usuarios no deben poder subir de su directorio personal (cada uno el suyo).
Configura el rango de puertos para el modo pasivo del 62000 al 63000.
Permite un máximo de 30 conexiones simultáneas, 5 de las cuales pueden ser anónimas.
Configura al usuario anónimo para que pueda descargar archivos existentes en el directorio /srv/public del servidor (debe configurar el directorio también).
Añade un certificado autofirmado siguiendo las indicaciones del manual que encontrará en el aula Moodle.
Configuración de un servidor SSH con certificados
En una máquina nueva, crea un usuario admin. En otra máquina (nueva o no), crea un usuario ubuntu. Logra que desde la máquina del usuario ubuntu se pueda acceder por SSH al usuario admin en la otra máquina sin necesidad de contraseña. Para ello, utiliza certificados autogenerados.
Comprueba que puedes acceder  con los certificados y documenta cómo lo pruebas.
Deshabilita el acceso por contraseña para el usuario admin cuando se utilice el servidor SSH.


## Comandos

Primero que nada, asegurarse de que todos los paquetes del sistema están actualizados:
sudo apt-get update
sudo apt-get upgrade


Instalación de proftpd:

sudo apt-get install proftpd
Durante la instalación nos preguntará sobre el tipo de instalación que queremos.
En caso de hacer poco uso del servidor FTP podemos seleccionar la opción «inetd» para ahorrar recursos del sistema.
Si vamos a hacer un uso considerable del servidor FTP con varios usuarios y bastante tiempo deberíamos instalarlo como «standalone» o «independientemente».
Esta configuración se podría modificar posteriormente, pero es recomendable seleccionar «standalone» porque se ejecutará como un servidor independiente, y en caso de tener un alto tráfico, funcionará mejor.


Configuración de proftpd:

Editamos el siguiente fichero:
sudo nano /etc/proftpd/proftpd.conf

Y en su contenido lo configuramos de esta manera:

ServerName: "provaserver"
DefaultRoot :  (ENTRE defaultroot y VIRULILLA HAY QUE PONER UN ESPACIO PARA QUE NO DÉ ERROR) descomentar esta linea para restringir el acceso de los usuarios a carpetas fuera de su home.

Reiniciar servicio proftpd para aplicar los cambios:
