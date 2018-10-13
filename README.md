SE exportaron los siguientes documentos de la actividad anterior: 
Dockerfile
create_dockers.sh
key.private
key.public
authorized_keys

Estando dentro de la carpeta ansible-to-migrate debemos ejecutar:

docker build -t server:16.04 .
../create_dockers.sh server:16.04

EL archivo create_dockers.sh fue editado evitando así el lanzamiento de un servidor que no usaremos.
Se crearán entonces dos que serán:
b67e4518c329        server:16.04        "/usr/sbin/sshd -D"   About an hour ago   Up About an hour    0.0.0.0:3306->3306/tcp, 0.0.0.0:2222->22/tcp   server02
61e37a394a18        server:16.04        "/usr/sbin/sshd -D"   About an hour ago   Up About an hour    0.0.0.0:2221->22/tcp, 0.0.0.0:8000->80/tcp     server01

Editar el archivo /etc/hosts, adicionando dos alias a localhost
127.0.0.1       server01 server02

Cambiamos los permisos de la llave privada:
chmod a-rwx ./key.private
chmod u+rw ./key.private

Ya podremos ejecutar:
ansible-playbook -i ./hosts ./site.yml 

En caso de obtener el siguiente error:
fatal: [server01]: FAILED! => {"changed": false, "cmd": "/usr/bin/git clone --origin origin https://github.com/bennojoy/mywebapp.git /var/www/html", "msg": "fatal: destination path '/var/www/html' already exists and is not an empty directory.", "rc": 128, "stderr": "fatal: destination path '/var/www/html' already exists and is not an empty directory.\n", "stderr_lines": ["fatal: destination path '/var/www/html' already exists and is not an empty directory."], "stdout": "", "stdout_lines": []}

Ingresar al contenedor por medio del siguiente comando:
ansible-playbook -i ./hosts ./site.yml 

Acceder al directorio /var/www/html y eliminar el archivo index.html que se encontrará allí.