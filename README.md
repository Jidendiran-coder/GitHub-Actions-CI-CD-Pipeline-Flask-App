# Flask Docker GitHub Actions Project

## Overview
This is a Flask web application configured with Docker and automated CI/CD using GitHub Actions. The project demonstrates a robust deployment pipeline with testing, building, and deployment to staging and production environments.

## Features
- Flask web application
- Docker containerization
- MongoDB integration
- Continuous Integration and Continuous Deployment (CI/CD)
- Automated testing
- Staging and Production deployment workflows

## Prerequisites
- Docker
- Docker Compose
- GitHub Account
- DockerHub Account

## Project Structure
```
.
├── .github/workflows/main.yml    # GitHub Actions workflow configuration
├── Dockerfile                    # Docker image build configuration
├── docker-compose.yml            # Docker Compose configuration
└── app.py                        # Flask application source code
```

## CI/CD Workflow
The GitHub Actions workflow (`main.yml`) implements the following stages:

### 1. Test Stage
- Runs on every push to `main` branch and for `prod` branch it will trigger on release tag push
- Builds and runs tests using Docker Compose
- Ensures code quality before proceeding to build

#### Test Stage Screenshot
<img width="1463" height="598" alt="1_test_success" src="https://github.com/user-attachments/assets/50dd4a63-23f0-46ce-8729-cb18150ae67e" />

#### Test Stage Console output
<img width="1453" height="702" alt="2_test_success_result" src="https://github.com/user-attachments/assets/63574aae-66ae-4b58-8a32-3b83e1573e9e" />

### 2. Build and Push Stage
- Builds Docker image
- Pushes image to DockerHub
- Tags image with git commit hash or tag

#### Build and Push Stage
<img width="1470" height="617" alt="3_build_and_push" src="https://github.com/user-attachments/assets/d6ad850f-a724-4cc7-b2cd-912139895295" />

### 3. Deployment Stages
#### Staging Deployment
- Triggered on pushes to `main` branch
- Deploys to a staging EC2 instance
- Sets up Docker network and MongoDB
- Pulls and runs the latest image

#### Output - Staging Deployment Success

<img width="1455" height="599" alt="4_staging_deployment_success" src="https://github.com/user-attachments/assets/bfd004b9-ffad-4177-b0dd-3ecd28aad579" />

#### Output - Staging Deployment Console Success

<img width="1444" height="650" alt="5_staging_deployment_sucess_details" src="https://github.com/user-attachments/assets/e5ce92aa-c9ac-4ef6-a2f0-556bab335084" />

#### Output - EC2 Containers Running with Images

<img width="1459" height="228" alt="6_docker_staging_containers_images" src="https://github.com/user-attachments/assets/1a473bd0-117f-4fa2-8f1c-e6222a1c827b" />

#### Output - Docker Staging Containers and Images

<img width="1283" height="319" alt="7_staging_success_overall" src="https://github.com/user-attachments/assets/2c65dce2-ccb1-4a97-96a0-ccc185b5936b" />


#### Production Deployment
- Triggered on version tags (e.g., `v1.0.0`)
- Deploys to production EC2 instance
- Similar setup to staging with production-specific configurations

#### Output - Production Deployment Success - without Staging Not Deployed

<img width="1274" height="315" alt="8_deploy_to_prod" src="https://github.com/user-attachments/assets/32e170bf-b91e-437a-9c28-2c5b4500717b" />

#### Output - Docker Staging Containers and Images

<img width="1455" height="737" alt="9_deploy_to_prod" src="https://github.com/user-attachments/assets/7d682577-8249-487f-bfc5-0422326ecb32" />

## Environment Variables
- `MONGO_URI`: MongoDB connection string
- `JWT_SECRET_KEY`: Secret key for JWT authentication
- `MONGO_DB_NAME`: MongoDB database name

## Docker Configuration
- Uses a custom Docker network (`flask-app-network`)
- Persistent MongoDB data volume
- Exposes application on port 8000

## Secrets Configuration
Configure the following secrets in your GitHub repository:
- `DOCKERHUB_USERNAME`: DockerHub username
- `DOCKERHUB_TOKEN`: DockerHub access token
- `EC2_HOST_IP`: Staging EC2 instance IP
- `PROD_EC2_HOST_IP`: Production EC2 instance IP
- `EC2_USER_NAME`: EC2 instance username
- `EC2_SSH_KEY`: SSH private key for EC2 access
- `JWT_SECRET_KEY`: Application JWT secret

## Local Development
1. Clone the repository
2. Create a `.env` file with necessary environment variables
3. Run `docker-compose up --build`

## Deployment
- Pushes to `main` trigger staging deployment
- Version tags (e.g., `v1.0.0`) trigger production deployment

