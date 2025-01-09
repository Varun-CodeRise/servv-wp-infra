# Setting Up WordPress and MySQL Using Docker Compose

Step-by-step guide to set up WordPress and MySQL using Docker Compose, along with solutions to the issues faced during the process.

---

## Requirement:-
1. Ubuntu- Install from microsoft store
2. Docker Desktop- Install from docker official site or using ubuntu
3. Git- install from official site or using ubuntu.

---

## Steps to Set Up

### Step 1: Create a Repository
1. Go to [GitHub](https://github.com) and create a repository named `servv-wp-infra`.
2. Clone the repository to your local machine:
   ```bash
   git clone <repository-url>
   cd servv-wp-infra
   ```

---

### Step 2: Create a Docker Compose File
1. Create a file named `docker-compose.yml` in the repository:
   ```bash
   nano docker-compose.yml
   ```
2. Add the following content:
   ```yaml
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
   ```
3. Save and exit.

---

### Step 3: Build and Run Docker Compose
#### **Command to Build Docker Compose**
Run the following command:
```bash
docker-compose build
```

#### **Error Encountered:**
```bash
WARN[0000] /home/varun/servv-wp-infra/docker-compose.yml: the attribute `version` is obsolete, it will be ignored, please remove it to avoid potential confusion
```

#### **Solution:**
1. Remove the `version` attribute from `docker-compose.yml`.
2. The updated file should look like this:
   ```yaml
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
   ```
3. Save the changes and rerun:
   ```bash
   docker-compose build
   ```

#### **Command to Run Docker Compose**
Run the following command to start the containers:
```bash
docker-compose up -d
```

#### **Error Encountered:**
```bash
unable to get image 'mysql:5.7': permission denied while trying to connect to the Docker daemon socket at unix:///var/run/docker.sock: Get "http://%2Fvar%2Frun%2Fdocker.sock/v1.47/images/mysql:5.7/json": dial unix /var/run/docker.sock: connect: permission denied
```

#### **Solution:**
1. Add your user to the `docker` group:
   ```bash
   sudo usermod -aG docker $USER
   ```
2. Restart your system or log out and log back in to apply the changes.
3. Verify Docker permissions:
   ```bash
   docker ps
   ```
   If it works without `sudo`, the issue is resolved.
4. Rerun the Docker Compose command:
   ```bash
   docker-compose up -d
   ```

---

### Step 4: Access WordPress Locally
1. Open your browser and navigate to:
   ```
   http://localhost:8080
   ```
2. Complete the WordPress setup wizard to configure the site.

---

### Step 5: Test WordPress
1. Log in to WordPress with the credentials you set.
2. Create a post under **Posts > Add New**.
   - Add a title and some content, then publish it.
3. Optional: Install plugins under **Plugins > Add New**.

---

### Step 6: Commit Changes to GitHub
1. Stage and commit your changes:
   ```bash
   git add .
   git commit -m "Add Docker Compose setup for WordPress and MySQL"
   ```
2. Push the changes to the feature branch:
   ```bash
   git push origin feature/setup-wp
   ```
3. Create a pull request and merge the changes into the `main` branch.

---

## Troubleshooting
1. **Permission Issues with Docker:**
   - Ensure your user is added to the `docker` group and has logged out and back in.
2. **WSL2 Integration Issues:**
   - Error encountered:
     ```bash
     The command 'docker' could not be found in this WSL 2 distro.
     We recommend to activate the WSL integration in Docker Desktop settings.
     ```
   - **Solution:**
     1. Open Docker Desktop settings.
     2. Navigate to **Resources > WSL Integration**.
     3. Enable WSL2 integration for your Linux distribution.
     4. Restart Docker Desktop and verify by running:
        ```bash
        docker --version
        ```
     5. If the issue persists, ensure WSL2 is properly installed and configured by following [Docker WSL2 setup documentation](https://docs.docker.com/go/wsl2/).

---
3. **Database Connection Issues:**
   - Ensure the `WORDPRESS_DB_*` environment variables match the MySQL credentials.

---
