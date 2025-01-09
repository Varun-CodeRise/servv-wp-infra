#Setting Up WordPress and MySQL Using Docker Compose

This document provides a step-by-step guide to set up WordPress and MySQL using Docker Compose, along with solutions to the issues faced during the process.

Prerequisites

Docker Desktop installed (on Windows, Mac, or Linux).

Git installed on your system.

Steps to Set Up

Step 1: Create a Repository

Go to GitHub and create a repository named servv-wp-infra.

Clone the repository to your local machine:

git clone <repository-url>
cd servv-wp-infra

Step 2: Create a Docker Compose File

Create a file named docker-compose.yml in the repository:

nano docker-compose.yml

Add the following content:

services:
  wordpress:
    image: wordpress:latest
    ports:
      - "8080:80"
    environment:
      WORDPRESS_DB_HOST: db
      WORDPRESS_DB_USER: user
      WORDPRESS_DB_PASSWORD: password
      WORDPRESS_DB_NAME: wordpress
    depends_on:
      - db

  db:
    image: mysql:5.7
    environment:
      MYSQL_ROOT_PASSWORD: rootpassword
      MYSQL_DATABASE: wordpress
      MYSQL_USER: user
      MYSQL_PASSWORD: password

Save and exit.

Step 3: Build and Run Docker Compose

Command to Build Docker Compose

Run the following command:

docker-compose build

Error Encountered:

WARN[0000] /home/varun/servv-wp-infra/docker-compose.yml: the attribute `version` is obsolete, it will be ignored, please remove it to avoid potential confusion

Solution:

Remove the version attribute from docker-compose.yml.

The updated file should look like this:

services:
  wordpress:
    image: wordpress:latest
    ports:
      - "8080:80"
    environment:
      WORDPRESS_DB_HOST: db
      WORDPRESS_DB_USER: user
      WORDPRESS_DB_PASSWORD: password
      WORDPRESS_DB_NAME: wordpress
    depends_on:
      - db

  db:
    image: mysql:5.7
    environment:
      MYSQL_ROOT_PASSWORD: rootpassword
      MYSQL_DATABASE: wordpress
      MYSQL_USER: user
      MYSQL_PASSWORD: password

Save the changes and rerun:

docker-compose build

Command to Run Docker Compose

Run the following command to start the containers:

docker-compose up -d

Error Encountered:

unable to get image 'mysql:5.7': permission denied while trying to connect to the Docker daemon socket at unix:///var/run/docker.sock: Get "http://%2Fvar%2Frun%2Fdocker.sock/v1.47/images/mysql:5.7/json": dial unix /var/run/docker.sock: connect: permission denied

Solution:

Add your user to the docker group:

sudo usermod -aG docker $USER

Restart your system or log out and log back in to apply the changes.

Verify Docker permissions:

docker ps

If it works without sudo, the issue is resolved.

Rerun the Docker Compose command:

docker-compose up -d

Step 4: Access WordPress Locally

Open your browser and navigate to:

http://localhost:8080

Complete the WordPress setup wizard to configure your site.

Step 5: Test WordPress

Log in to WordPress with the credentials you set.

Create a post under Posts > Add New.

Add a title and some content, then publish it.

Optional: Install plugins under Plugins > Add New.

Step 6: Commit Changes to GitHub

Stage and commit your changes:

git add .
git commit -m "Add Docker Compose setup for WordPress and MySQL"

Push the changes to the feature branch:

git push origin feature/setup-wp

Create a pull request and merge the changes into the main branch.

Troubleshooting

Permission Issues with Docker:

Ensure your user is added to the docker group and has logged out and back in.

Ports Already in Use:

If port 8080 is in use, modify the ports section in docker-compose.yml to another port, e.g., 8081:80.

ports:
  - "8081:80"

Database Connection Issues:

Ensure the WORDPRESS_DB_* environment variables match the MySQL credentials.

