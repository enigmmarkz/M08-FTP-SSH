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
DefaultRoot :  (ENTRE defaultroot y VIRULILLA HAY QUE PONER UN ESPACIO PARA QUE NO DÉ ERROR) descomentar esta linea para restringir los usuarios con sus carpetas de usuario (home)








