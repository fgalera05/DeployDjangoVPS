# DeployDjangoVPS
Poner en producción Django con Apache con WSGI

## Detalle de lo que se quiere poner en producción:
  - Desarrollo realizado en Django con repositorio en GitHub
  - Base de datos MySQL

## Requisitos:
  - Servidor con Ubuntu Server con acceso sobre HTTP, HTPPS y SSH.
  - nombre_de_dominio
  - Archivo de SQL para importar: mi_archivo.sql

### 1) Apache:
  - sudo apt update
  - sudo apt upgrade
  - sudo apt install apache2

2) UFW (Recomendado):
  - sudo apt install ufw
  - sudo ufw allow ssh
  - sudo apt install apache2
  - sudo ufw allow in "Apache Full"
  - sudo ufw enable

*** En este punto, ya deberías de poder ver la página principal de Apache en tu navegador ***

3) Git
  - sudo apt install git-all
  - Vamos a la carpeta de Apache para clonar el repo:
    - cd /var/www/html/
    - Deberíamos de lograr tener: /var/www/html/nombre_repositorio

4) Entorno virtual:
  - cd /nombre_repositorio
  - sudo pip3 install virtualenv 
  - virtualenv -p python3 venv
  - source env/bin/activate
  - pip install -r requirements.txt

5) Opcional ya que depende de tu requirements.txt:
  - sudo apt-get install python3-dev default-libmysqlclient-dev build-essential
  - pip3 install mysqlclient
  - pip3 install django
  - deactivate

6) WSGI para Apache
  - sudo apt-get install libapache2-mod-wsgi-py3 
  - sudo a2enmod wsgi

7) MySQL
  - sudo apt-get update
  - sudo apt-get install mysql-server 
  - sudo mysql_secure_installation (opcional)
  - sudo systemctl start mysql
  - sudo systemctl restart mysql

8) Entramos en mysql:
  - > CREATE USER 'your_username'@'LOCALHOST' IDENTIFIED BY 'your_password';
  - > CREATE DATABASE my_db;
  - > GRANT ALL PRIVILEGES ON my_db.* TO 'your_username'@'LOCALHOST';
  - > FLUSH PRIVILEGES;
  - > EXIT;

9) Crear sitio web en Apache:
  - sudo chmod -R 755 /var/www
  - sudo cp /etc/apache2/sites-available/000-default.conf /etc/apache2/sites-available/nombre_de_dominio.conf 
  - sudo a2ensite nombre_de_dominio.conf
  - sudo a2dissite 000-default.conf 

10) Conetar Django con sitio web:
  - sudo nano /etc/apache2/sites-available/nombre_de_dominio.conf
  - Configuración: ver ejemploVirtualHost.txt
  - sudo apache2ctl configtest
  - sudo systemctl status apache2
  - sudo systemctl restart apache2

*** Con esta configuración ya debería de funcionar tu página principal del proyecto ***
*** Para cualquier error recurrir a los logs en: cd /var/log/apache2/ ***

11) Volcado de información en MySQL:
  - mysqldump -u your_username -p my_db < mi_archivo.sql

12) Configurar tu settings.py

13) Certificado:
  - sudo apt install certbot python3-certbot-apache
  - sudo certbot --apache (En las opciones se puede elegir redireccionar HTTP a HTTPS)
  - sudo systemctl restart apache2

14) BackUp MySQL:
  - De la forma que lo hagas vas a necesitar agregar una regla si instalaste el UFW:
    - sudo ufw allow from "ip" to any port 3306
    - Permisos necesarios si no los configuraste en 8)



