# Jenkins Setup with Docker Compose

This repository contains a `docker-compose.yml` file for setting up Jenkins with Docker. This setup includes the necessary configurations to map ports and persist Jenkins data using Docker volumes.

## Prerequisites

Ensure you have the following installed on your machine:
- Docker
- Docker Compose

## Getting Started

### Step 1: Clone the Repository

Clone this repository to your local machine:

```bash
git clone https://github.com/yourusername/jenkins-docker-compose.git
cd jenkins-docker-compose
```

### Step 2: Create and Start Jenkins Container

Run the following command to start Jenkins using Docker Compose:

```bash
docker-compose up -d
```

The `-d` flag runs the containers in detached mode.

### Step 3: Access Jenkins

Open your web browser and navigate to `http://localhost:8080`. You will see the Jenkins setup screen.

### Step 4: Initial Setup

#### Unlock Jenkins

Jenkins will prompt you to unlock it using an initial admin password. Retrieve this password from the container logs:

```bash
docker-compose logs jenkins
```

Look for a log entry similar to this:

```
*************************************************************
*************************************************************
*************************************************************

Jenkins initial setup is required. An admin user has been created and a password generated.

Please use the following password to proceed to installation:

abc123def456ghi789jkl

This may also be found at: /var/jenkins_home/secrets/initialAdminPassword

*************************************************************
*************************************************************
*************************************************************
```

Copy the password and paste it into the Jenkins setup page.

#### Install Suggested Plugins

Jenkins will prompt you to install plugins. Choose "Install suggested plugins" for a standard setup.

#### Create First Admin User

Follow the on-screen instructions to create your first admin user.

#### Instance Configuration

Confirm the Jenkins URL and finish the setup.

## Managing Jenkins

### Stop Jenkins

To stop the Jenkins service, run:

```bash
docker-compose down
```

### Start Jenkins

To start the Jenkins service again, run:

```bash
docker-compose up -d
```

### View Logs

To view the logs, run:

```bash
docker-compose logs -f
```

### Remove Jenkins Service and Data

To remove the Jenkins service and its data, run:

```bash
docker-compose down -v
```

## Directory Structure

```
jenkins-docker-compose/
│
├── docker-compose.yml
└── README.md
```

- `docker-compose.yml`: Docker Compose file for setting up Jenkins.
- `README.md`: This readme file.

## Configuration Details

### Docker Compose File

```yaml
version: '3.8'

services:
  jenkins:
    image: jenkins/jenkins:lts
    container_name: jenkins
    ports:
      - "8080:8080"   # Jenkins web interface
      - "50000:50000" # Jenkins agent communication
    volumes:
      - jenkins_home:/var/jenkins_home
    restart: unless-stopped

volumes:
  jenkins_home:
```

- **version**: Specifies the Docker Compose file format version.
- **services**: Defines the services to be run.
  - **jenkins**: The name of the Jenkins service.
    - **image**: Specifies the Docker image to use (`jenkins/jenkins:lts`).
    - **container_name**: Names the container `jenkins`.
    - **ports**: Maps ports from the host to the container.
      - `"8080:8080"`: Maps port 8080 on the host to port 8080 in the container for the Jenkins web interface.
      - `"50000:50000"`: Maps port 50000 on the host to port 50000 in the container for Jenkins agent communication.
    - **volumes**: Mounts a Docker volume to persist Jenkins data.
      - `jenkins_home:/var/jenkins_home`: Creates a volume named `jenkins_home` and mounts it to `/var/jenkins_home` in the container.
    - **restart**: Ensures the container restarts automatically unless it is explicitly stopped.

## Conclusion

By following these steps, you will have a fully functional Jenkins instance running in a Docker container. You can further customize Jenkins by installing additional plugins and configuring it according to your CI/CD needs.
```

Feel free to adjust the repository URL and any other details as necessary.
