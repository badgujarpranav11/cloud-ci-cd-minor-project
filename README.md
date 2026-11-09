# CI/CD Pipeline using GitHub Actions

## Topic
Automation of Infrastructure and CI/CD Pipelines

## Tools
- GitHub Actions
- Docker
- Docker Hub
- Python Flask

## Project Overview
This project demonstrates an automated CI/CD pipeline.
When code is pushed to the main branch, GitHub Actions
builds a Docker image and pushes it to Docker Hub.

## Setup
1. Create a GitHub repository.
2. Create a Docker Hub repository named cloud-ci-cd-demo.
3. Generate a Docker Hub access token with read and write permissions.
4. Add DOCKERHUB_USERNAME and DOCKERHUB_TOKEN as GitHub Actions secrets.
5. Push this project to the main branch.
6. Open the Actions tab to monitor the workflow.

## Expected Result
A successful workflow builds and publishes the Docker image
to Docker Hub.

The project is complete only after the workflow succeeds
and the image is visible in Docker Hub.