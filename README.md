docker run -d --name mysql -v mysql-data:/var/lib/mysql --network=python-netwrok -e MYSQL_DATABASE=mydb -e MYSQL_ROOT_PASSWORD=admin -p 3306:3306 mysql:5.7

docker run -d --name flaskapp --network=python-netwrok -e MYSQL_HOST=mysql -e MYSQL_USER=root -e MYSQL_PASSWORD=admin -e MYSQL_DB=mydb -p 1111:5000 python-backend

CREATE TABLE messages (id INT AUTO_INCREMENT PRIMARY KEY,message TEXT);

