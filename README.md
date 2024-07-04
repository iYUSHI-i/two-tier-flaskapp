 
# Flask App with MySQL Docker Setup

This project is a straightforward Flask application that interfaces with a MySQL database. Users can submit messages, which are stored in the database and displayed on the frontend. This setup is designed to demonstrate the integration of Flask with MySQL using Docker, providing a robust and scalable development environment.


## Setup

1. Clone this repository :

   ```bash
   git clone https://github.com/your-username/your-repo-name.git
   ```

   Cloning the repository ensures you have the latest version of the codebase to work with.

2. Change to the project directory:

   ```bash
   cd your-repo-name
   ```

3. Create a .env file in the project directory to store your MySQL environment variables:


   ```bash
   touch .env
   ```

4. Open the `.env` file and add your MySQL configuration:

   ```
   MYSQL_HOST=mysql
   MYSQL_USER=your_username
   MYSQL_PASSWORD=your_password
   MYSQL_DB=your_database
   ```

## Usage

1. Launch the containers using Docker Compose:

   ```bash
   docker-compose up --build
   ```

2. Access the Flask app in your web browser:

   - Frontend: http://localhost
   - Backend: http://localhost:5000

3. Create the `messages` table in your MySQL database:

   - Use a MySQL client or tool (e.g., phpMyAdmin) to execute the following SQL commands:
   
     ```sql
     CREATE TABLE messages (
         id INT AUTO_INCREMENT PRIMARY KEY,
         message TEXT
     );
     ```

4. Interact with the app:

  - Visit http://localhost to see the frontend and submit new messages using the form.
  - Visit http://localhost:5000/insert_sql to insert a message directly into the messages table    via an SQL query.
These interactions demonstrate the full cycle of data flow from the user interface to the database and back.


## To run this two-tier application using  without docker-compose

- First create a docker image from Dockerfile
```bash
docker build -t flaskapp .
```

- Now, make sure that you have created a network using following command
```bash
docker network create twotier
```

- Attach both the containers in the same network, so that they can communicate with each other

i) MySQL container 
```bash
docker run -d \
    --name mysql \
    -v mysql-data:/var/lib/mysql \
    --network=twotier \
    -e MYSQL_DATABASE=mydb \
    -e MYSQL_USER=root \
    -e MYSQL_ROOT_PASSWORD=admin \
    -p 3306:3306 \
    mysql:5.7

```
ii) Backend container
```bash
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

### Inspiration
Tutor: https://github.com/LondheShubham153


