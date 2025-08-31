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
![Test Stage Success](/output/1_test_success.png)

#### Test Stage Console output
![Deployment Stage Success](/output/2_test_success_result.png)

### 2. Build and Push Stage
- Builds Docker image
- Pushes image to DockerHub
- Tags image with git commit hash or tag

#### Build and Push Stage
![Build and Push Stage](/output/3_build_and_push.png)

### 3. Deployment Stages
#### Staging Deployment
- Triggered on pushes to `main` branch
- Deploys to a staging EC2 instance
- Sets up Docker network and MongoDB
- Pulls and runs the latest image

#### Output - Staging Deployment Success

![Staging Deployment Stage](/output/4_staging_deployment_success.png)

#### Output - Staging Deployment Console Success

![Staging Deployment Stage](/output/5_staging_deployment_sucess_details.png)

#### Output - EC2 Containers Running with Images

![Docker Staging Containers and Images](/output/6_docker_staging_containers_images.png)

#### Output - Docker Staging Containers and Images

![Staging Deployment Overall Success](/output/7_staging_success_overall.png)


#### Production Deployment
- Triggered on version tags (e.g., `v1.0.0`)
- Deploys to production EC2 instance
- Similar setup to staging with production-specific configurations

#### Output - Production Deployment Success - without Staging Not Deployed

![Production Deployment Stage](/output/8_deploy_to_prod.png)

#### Output - Docker Staging Containers and Images

![Production Deployment Docker Containers and Images](/output/9_deploy_to_prod.png)


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

