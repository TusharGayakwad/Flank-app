docker run -d --name mysql -v mysql-data:/var/lib/mysql --network=python-netwrok -e MYSQL_DATABASE=mydb -e MYSQL_ROOT_PASSWORD=admin -p 3306:3306 mysql:5.7

docker run -d --name flaskapp --network=python-netwrok -e MYSQL_HOST=mysql -e MYSQL_USER=root -e MYSQL_PASSWORD=admin -e MYSQL_DB=mydb -p 1111:5000 python-backend

CREATE TABLE messages (id INT AUTO_INCREMENT PRIMARY KEY,message TEXT);



apiVersion: apps/v1
kind: Deployment
metadata:
  name: mysql
  labels:
    app: db
spec:
  replicas: 1
  selector:
    matchLabels:
      app: db
  template:
    metadata:
      labels:
        app: db
    spec:
      containers:
      - name: mysql
        image: mysql
        imagePullPolicy: Never
        env:
        - name: MYSQL_ROOT_PASSWORD
          valueFrom:
            secretKeyRef:
              name: flaskapi-secrets
              key: db_root_password
        ports:
        - containerPort: 3306
          name: db-container
        volumeMounts:
          - name: mysql-persistent-storage
            mountPath: /var/lib/mysql
      volumes:
        - name: mysql-persistent-storage
          persistentVolumeClaim:
            claimName: mysql-pv-claim


---
apiVersion: v1
kind: Service
metadata:
  name: mysql
  labels:
    app: db
spec:
  ports:
  - port: 3306
    protocol: TCP
    name: mysql
  selector:
    app: db
  type: NodePort