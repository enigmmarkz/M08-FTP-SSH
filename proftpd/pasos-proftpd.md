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
UseIPV6: si no se usa ponerlo en off
DefaultRoot :  (ENTRE defaultroot y VIRULILLA HAY QUE PONER UN ESPACIO PARA QUE NO DÉ ERROR) descomentar esta linea para restringir los usuarios con sus carpetas de usuario
RequireValidShell: Uncomment this line and make it “On” to enable logging in for users, even for those who doesn’t have a valid shell in /etc/shells to log in.
AuthOrder: Uncomment the line to enable the using of local passwords.
Port: This line defines the default port for the FTP server, it is 21 by default. If you want, you can define any custom port here.
SystemLog: The default log file path, you may change it if you want.









