## Instal·lació d'un servidor Nginx en Linux Ami 2023 (Fedora)

```
dnf update -y
dnf install nginx -y
systemctl start nginx
systemctl enable nginx
systemctl status nginx
cd /usr/share/nginx/html
rm index.html
wget https://sanvalero-static-webs.s3.eu-west-1.amazonaws.com/breakout.zip
unzip breakout.zip
```

## Instal·lació d'un servidor Apache en Ubuntu 

```
sudo apt update -y
sudo apt install -y apache2
sudo systemctl start apache2
sudo systemctl enable apache2
echo "<h1>Benvingut a la meua web d'Apache en Ubuntu en una EC2</h1>" | sudo tee /var/www/html/index.html
```


## Instal·lació de PHP sobre Apache en Ubuntu

```
sudo apt update -y
sudo apt install -y php libapache2-mod-php php-mysql
sudo systemctl restart apache2
```

## Instal·lació i configuració de MySQL sobre Ubuntu

```
sudo apt update -y
sudo apt install -y mysql-server
sudo systemctl start mysql
sudo systemctl enable mysql
sudo systemctl status mysql

#Creació d'usuari per permetres conexions remotes (no solo desde localhost)
# Entrar a MySQL com a root (en Ubuntu pot ser sense password)
sudo mysql

# Crear usuari que es puga connectar des de la xarxa privada corresponent (exemple: subnet 10.0.0.0/16)
CREATE USER 'appuser'@'10.0.%' IDENTIFIED BY 'ContrasenyaSegura123';

# Donar-li privilegis sobre una base de dades concreta
CREATE DATABASE appdb;
GRANT ALL PRIVILEGES ON appdb.* TO 'appuser'@'10.0.%';

# Actualitzar
FLUSH PRIVILEGES;
EXIT;

#Configurar MySQL per admetre connexions remotes
sudo nano /etc/mysql/mysql.conf.d/mysqld.cnf

#Canviar bind-address per la direcció IP des d'on ens connectarem (o tots)
bind-address = 0.0.0.0

#Reiniciar
sudo systemctl restart mysql
```

No cal olvidar que cal permetre explicitament les connexions al port de MySQL, el **3306**.

## Configuració per a connectar-se a BBDD MySQL
Desde la màquina client (des d'on volem connectar a la base de dades):

```
sudo apt install -y mysql-client
mysql -h <IP_privada_mysql> -u appuser -p
```

Si entra correctament, està funcionant 👍🏼

## Conectar per PHP a una BBDD MySQL

A la instància del servidor web+php, crearem un fitxer php, per exemple *dbtest.php*:
````
sudo nano /var/www/html/dbtest.php
````

Contingut del fitxer *dbtest.php*:

```
<?php
$servername = "IP_PRIVADA_MYSQL";   // o hostname dins de la VPC
$username = "appuser";              // Usuari que has creat
$password = "ContrasenyaSegura123"; // Contrasenya que has triat
$dbname = "appdb";                  // Base de dades creada

// Crear connexió
$conn = new mysqli($servername, $username, $password, $dbname);

// Comprovar connexió
if ($conn->connect_error) {
  die("Connexió fallida: " . $conn->connect_error);
}
echo "<h2>Connexió a MySQL correcta! 🎉</h2>";

// Opcional: crear una taula i inserir dades si no existeix
$sql = "CREATE TABLE IF NOT EXISTS proves (
  id INT AUTO_INCREMENT PRIMARY KEY,
  nom VARCHAR(50) NOT NULL
)";
$conn->query($sql);

// Inserir un registre de prova
$conn->query("INSERT INTO proves (nom) VALUES ('Hola des de PHP')");

// Llegir dades
$result = $conn->query("SELECT id, nom FROM proves");

echo "<h3>Dades llegides de la taula 'proves':</h3>";
if ($result->num_rows > 0) {
  while($row = $result->fetch_assoc()) {
    echo "ID: " . $row["id"]. " - Nom: " . $row["nom"]. "<br>";
  }
} else {
  echo "Taula buida.";
}

$conn->close();
?>
```

Guardem el fitxer i ens assegurem que té els permisos adequats:

```
sudo chown www-data:www-data /var/www/html/dbtest.php
sudo chmod 644 /var/www/html/dbtest.php
sudo systemctl restart apache2
```

I des del navegador accedim *http://IP_PÚBLICA_DE_LA_WEB/dbtest.php*

