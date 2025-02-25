# 🚀 Flask App with MySQL - Docker Setup

This is a simple Flask app that interacts with a MySQL database. The app allows users to submit messages, which are then stored in the database and displayed on the frontend.

## 🛠 Prerequisites
Before you begin, make sure you have the following installed:

- [Docker](https://www.docker.com/)
- [Git](https://git-scm.com/) (optional, for cloning the repository)

---

## 📂 Setup
### 1️⃣ Clone this repository
```sh
git clone https://github.com/your-username/your-repo-name.git
cd your-repo-name
```

### 2️⃣ Configure MySQL Environment Variables
Create a `.env` file in the project directory:
```sh
touch .env
```
Edit `.env` and add the following configuration:
```env
MYSQL_HOST=mysql
MYSQL_USER=your_username
MYSQL_PASSWORD=your_password
MYSQL_DB=your_database
```

### 3️⃣ Start the Containers using Docker Compose
```sh
docker-compose up --build
```

### 4️⃣ Access the Application
- **Frontend**: [http://localhost](http://localhost)
- **Backend**: [http://localhost:5000](http://localhost:5000)

### 5️⃣ Create the Database Table
Use a MySQL client or tool (e.g., phpMyAdmin) to execute the following SQL command:
```sql
CREATE TABLE messages (
    id INT AUTO_INCREMENT PRIMARY KEY,
    message TEXT
);
```

### 6️⃣ Interact with the App
- Visit **[http://localhost](http://localhost)** to submit new messages.
- Visit **[http://localhost:5000/insert_sql](http://localhost:5000/insert_sql)** to insert a message directly via an SQL query.

---

## 🐳 Running Without `docker-compose`
If you prefer not to use `docker-compose`, follow these steps:

### 1️⃣ Build the Flask App Docker Image
```sh
docker build -t flaskapp .
```

### 2️⃣ Create a Docker Network
```sh
docker network create twotier
```

### 3️⃣ Run MySQL Container
```sh
docker run -d \
    --name mysql \
    -v mysql-data:/var/lib/mysql \
    --network=twotier \
    -e MYSQL_DATABASE=mydb \
    -e MYSQL_ROOT_PASSWORD=admin \
    -p 3306:3306 \
    mysql:5.7
```

### 4️⃣ Run Flask App Container
```sh
docker run -d \
    --name flaskapp \
    --network=twotier \
    -e MYSQL_HOST=mysql \
    -e MYSQL_USER=root \
    -e MYSQL_PASSWORD=admin \
    -e MYSQL_DB=mydb \
    -p 5000:5000 \
    flaskapp:latest
```

---

## 🧹 Cleaning Up
To stop and remove the containers, use:
```sh
docker-compose down
```

If running without `docker-compose`, stop containers manually:
```sh
docker stop flaskapp mysql
```
And remove them:
```sh
docker rm flaskapp mysql
```

---

## 🔥 Notes & Best Practices
✅ Replace placeholders (`your_username`, `your_password`, `your_database`) with actual values.
✅ Follow best security practices for production deployments.
✅ Sanitize user inputs to prevent SQL injection.
✅ Check Docker logs for troubleshooting:
```sh
docker logs flaskapp
docker logs mysql


