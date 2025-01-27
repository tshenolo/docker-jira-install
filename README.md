# Docker Jira Install

## Overview
This repository provides a simple and efficient way to deploy Atlassian Jira Software using Docker Compose. With this setup, Jira runs in a containerized environment with PostgreSQL as the database, making it easy to deploy and manage.

## Features
- **Easy Deployment**: Spin up Jira Software with just a few commands.
- **PostgreSQL Integration**: Uses PostgreSQL as the database backend.
- **Persistent Storage**: Data is stored persistently using Docker volumes.
- **Customizable Configuration**: Modify environment variables to suit your needs.

## Prerequisites
Ensure you have the following installed on your system:
- [Docker](https://www.docker.com/get-started)
- [Docker Compose](https://docs.docker.com/compose/install/)

## Installation

### Option 1: Clone the Repository
Clone this repository to get started quickly:

```bash
git clone https://github.com/tshenolo/docker-jira-install.git
```

Navigate into the project
```bash
cd docker-jira-install
```

### Option 2: Create the Required Files Manually
If you prefer, you can manually create the required files using the contents provided below.

Create docker-compose.yml with the following content:
```yaml
version: '3.8'

services:
  postgres:
    image: postgres:13
    container_name: jira_postgres
    environment:
      POSTGRES_DB: jiradb
      POSTGRES_USER: ${POSTGRES_USER}
      POSTGRES_PASSWORD: ${POSTGRES_PASSWORD}
    volumes:
      - postgres_data:/var/lib/postgresql/data
    networks:
      - jira_network

  jira:
    image: atlassian/jira-software:latest
    container_name: jira
    depends_on:
      - postgres
    environment:
      ATL_JDBC_URL: jdbc:postgresql://postgres:5432/jiradb
      ATL_JDBC_USER: ${POSTGRES_USER}
      ATL_JDBC_PASSWORD: ${POSTGRES_PASSWORD}
      ATL_DB_DRIVER: org.postgresql.Driver
    ports:
      - "8080:8080"
    volumes:
      - jira_data:/var/atlassian/application-data/jira
    networks:
      - jira_network

volumes:
  postgres_data:
  jira_data:

networks:
  jira_network:
```

Create a .env file in the same directory with the following content:
```env
POSTGRES_USER=jirauser
POSTGRES_PASSWORD=jirapassword
```

### Usage
#### Step 1: Start the Jira Containers
Once everything is set up, run the following command to start Jira and PostgreSQL containers:
```bash
docker-compose up -d
```

#### Step 2: Access Jira
Once the containers are running, open your web browser and navigate to:
```
http://localhost:8080/
```

#### Step 3: Configure Jira
Follow these steps in the web interface to set up Jira:

1. Database Setup

- Select PostgreSQL as the database type.
- Use the following credentials:
    - Hostname: postgres
    - Port: 5432
    - Database: jiradb
    - Username: jirauser
    - Password: jirapassword
    - Schema: public

2. Application Setup

- Set application title to Jira
- Mode: Private
- Base URL: http://localhost:8080

3. License Key

- Obtain a Jira trial license from MyAtlassian and paste it when prompted.

4. Administrator Account

- Create an admin account with your details.

5. Email Notifications

- Choose to configure later if not needed initially.

6. Welcome to Jira

- Select language, avatar, and create a sample project.

#### Stopping and Removing Containers
To stop the running Jira containers, use:
```bash
docker-compose down
```

To remove all related volumes:
```bash
docker-compose down -v
```

#### Troubleshooting
If you encounter any issues, check the logs using:
```bash
docker-compose logs -f
```

## License
This project is licensed under the terms of the MIT License.

## Contributing
Contributions are welcome! Feel free to submit issues or pull requests to improve this project.










